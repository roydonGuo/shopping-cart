<template>
  <div class="app-container">
    <!-- <h1>App 根组件</h1> -->
    <Header title="购物车"></Header>
    <Goods
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :goods_name="item.goods_name"
      :goods_img="item.goods_img"
      :goods_price="item.goods_price"
      :goods_state="item.goods_state"
      :goods_count="item.goods_count"
      @state-changed="getNowState"
    ></Goods>
    <Footer
      :isFull="footerState"
      @isAll="allSelect"
      :amt="amt"
      :total="total"
    ></Footer>
  </div>
</template>

<script>
import axios from "axios";
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
import bus from "@/components/eventBus.js";

export default {
  data() {
    return {
      list: [],
    };
  },
  created() {
    this.initCartData();
    bus.$on("share", (obj) => {
      this.list.some((item) => {
        if (item.id === obj.id) {
          item.goods_count = obj.num;
          return true;
        }
      });
    });
    // bus.$on("reduce", (obj) => {
    //   this.list.some((item) => {
    //     if (item.id === obj.id) {
    //       item.goods_count = obj.num;
    //       return true;
    //     }
    //   });
    // });
  },
  computed: {
    // 计算属性下的都是当作值直接传递
    footerState() {
      return this.list.every((item) => item.goods_state);
    },
    // 已勾选商品的总价值
    amt() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce(
          (total, item) => (total += item.goods_price * item.goods_count),
          0
        );
    },
    total() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((total, item) => (total += item.goods_count), 0);
    },
  },
  methods: {
    //初始化购物车数据
    async initCartData() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      console.log(res);
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    getNowState(obj) {
      // obj 就是子组件传来的新状态 {id: 1, state: false}...
      console.log(obj);
      this.list.some((item) => {
        if (item.id == obj.id) {
          item.goods_state = obj.state;
          // 终止后续不必要的循环
          return true;
        }
      });
    },
    allSelect(e) {
      // console.log(e);
      this.list.forEach((item) => {
        item.goods_state = e;
      });
    },
  },
  components: {
    Header,
    Goods,
    Footer,
  },
};
</script>
<!-- https://www.escook.cn/api/cart -->
<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
