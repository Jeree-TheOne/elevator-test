<template>
  <div class="h-full relative">
    <div class="container">
      <span>{{ floor }}</span>
      <button type="button" @click="$emit('called', floor)">
        <div></div>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    floor: {
      Type: Number,
      required: true,
    },
    active: {
      type: Boolean,
      required: true,
    },
  },

  computed: {
    color() {
      return this.active ? "rgb(255, 166, 0)" : "rgb(131, 104, 226)";
    },
  },
};
</script>

<style scoped lang="scss">
.container {
  top: 50%;
  transform: translateY(-50%);
  position: absolute;
}
button {
  $color: v-bind(color);
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  & div {
    width: 5px;
    height: 5px;
    background-color: $color;
    border-radius: 1000px;
    box-shadow: 0 0 0 5px rgb(255, 255, 255), 0 0 0 10px $color;
  }
}
</style>
