<template>
  <div
    class="number-container d-flex justify-content-center align-items-center"
  >
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="reduceCount">
      -
    </button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ count }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="addCount">
      +
    </button>
  </div>
</template>

<script>
import bus from "@/components/eventBus.js";

export default {
  props: {
    id: {
      required: true,
      type: Number,
    },
    count: {
      default: 1,
      type: Number,
    },
  },
  methods: {
    addCount() {
      var obj = { id: this.id, num: this.count + 1 };
      console.log("'+'---id:" + obj.id + "---" + "value:" + obj.num);
      bus.$emit("share", obj);
    },
    reduceCount() {
      if (this.count > 1) {
        var obj = { id: this.id, num: this.count - 1 };
        console.log("'-'---id:" + obj.id + "---" + "value:" + obj.num);
        bus.$emit("share", obj);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
