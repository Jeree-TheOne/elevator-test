<template>
  <div class="shaft">
    <div class="cell" ref="cell"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentFloor: 1,
      isOpen: true,
    };
  },

  mounted() {
    this.moveToFloor();
  },

  methods: {
    moveToFloor() {},
  },

  computed: {
    height() {
      return 100 / process.env.VUE_APP_BTN_AMOUNT + "%";
    },
  },
};
</script>

<style scoped>
.shaft {
  position: relative;
  height: 100%;
  width: 120px;
}
.cell {
  bottom: 0;
  position: absolute;
  border: 2px solid black;
  background-color: rgb(53, 176, 233);
  height: v-bind(height);
  width: 100%;
}
</style>
