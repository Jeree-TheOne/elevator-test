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

  // mounted() {
  //   this.moveToFloor(5);
  // },

  methods: {
    moveToFloor(floor) {
      if (this.currentFloor == floor) return;
      this.isOpen = false;
      let duration = Math.abs(floor - this.currentFloor) * 1000;
      let finalPossition = `translateY(${(floor - 1) * -100 + "%"})`;
      this.$refs.cell.animate(
        [
          { transform: this.$refs.cell.transform },
          {
            transform: finalPossition,
          },
        ],
        {
          duration,
          iterations: 1,
        }
      );
      let s = setInterval(() => {
        this.moveFloorNumber(floor);
      }, 1000);
      setTimeout(() => {
        clearInterval(s);
        this.$refs.cell.style.transform = finalPossition;
        this.$refs.cell.animate(
          [
            { opacity: "1" },
            { opacity: "0.5" },
            {
              opacity: "1",
            },
          ],
          {
            duration: 1000,
            iterations: 3,
          }
        );
        setTimeout(() => {
          this.isOpen = true;
          this.$emit("open", this.currentFloor);
        }, 3000);
      }, duration);
    },

    moveFloorNumber(floor) {
      this.currentFloor > floor ? this.currentFloor-- : this.currentFloor++;
    },
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
  border-left: 3px solid #ccc;
  border-right: 3px solid #ccc;
  margin: 0 7px;
}
.cell {
  bottom: 0;
  transform: translateY(0);
  position: absolute;
  border: 2px solid black;
  background-color: rgb(53, 176, 233);
  height: v-bind(height);
  width: 100%;
}
</style>
