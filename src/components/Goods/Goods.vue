<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input
          type="checkbox"
          class="custom-control-input"
          :id="'cb' + id"
          :checked="goods_state"
          @change="stateChanged"
        />
        <label class="custom-control-label" :for="'cb' + id">
          <!-- 商品的缩略图 -->
          <img :src="goods_img" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ goods_name }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ goods_price }}</span>
        <!-- 商品的数量 -->
        <Counter :count="goods_count" :id="id"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from "@/components/Counter/Counter.vue";

export default {
  props: {
    id: {
      required: true,
      type: Number,
    },
    goods_name: {
      default: "",
      type: String,
    },
    goods_img: {
      default: "",
      type: String,
    },
    goods_price: {
      default: "",
      type: Number,
    },
    goods_state: {
      default: true,
      type: Boolean,
    },
    goods_count: {
      default: 1,
      type: Number,
    },
  },
  methods: {
    stateChanged(e) {
      const nowState = e.target.checked;
      this.$emit("state-changed", { id: this.id, state: nowState });
    },
  },
  components: {
    Counter,
  },
};
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
