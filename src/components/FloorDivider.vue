<template>
  <div>
    <hr v-for="index in floors" :key="index" />
  </div>
</template>

<script>
export default {
  props: {
    floors: {
      Number,
      default: 1,
    },
  },

  computed: {
    margin() {
      return 100 / this.floors + "vh";
    },
  },
};
</script>

<style scoped>
div {
  height: 100vh;
  width: 100%;
  position: absolute;
}
hr {
  width: 100%;
  margin-top: calc(v-bind(margin) - 1px);
}
</style>
