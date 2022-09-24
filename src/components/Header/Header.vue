<template>
  <div class="header-container">{{ title }}</div>
</template>

<script>
export default {
  props: {
    title: {
      default: "",
      type: String,
    },
  },
};
</script>

<style lang="less" scoped>
.header-container {
  font-size: 12px;
  height: 45px;
  width: 100%;
  background-color: #1d74ff;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  position: fixed;
  top: 0;
  z-index: 999;
}
</style>
