<template>
  <div class="shaft">
    <div class="cell" ref="cell">
      <div class="indicator" v-if="!isOpen">
        <div :style="[direction ? '' : 'transform: rotate(180deg)']">▲</div>
        <div>{{ floorToGo }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import { nextTick } from "@vue/runtime-core";
export default {
  data() {
    return {
      currentFloor: 1,
      isOpen: true,
      floorToGo: 1,
      direction: true,
    };
  },

  props: {
    startingFloor: {
      type: Number,
      default: 1,
    },
    finalFloor: {
      type: Number,
      default: 0,
    },
  },

  mounted() {
    nextTick(() => {
      this.currentFloor = this.startingFloor;
      this.floorToGo = this.finalFloor;

      this.$refs.cell.style.transform = `translateY(${
        (this.currentFloor - 1) * -100 + "%"
      })`;
      if (this.currentFloor == this.floorToGo) this.floorToGo = 0;
      if (this.floorToGo > 0) this.moveToFloor(this.floorToGo);
    });
  },

  methods: {
    moveToFloor(floor) {
      if (this.currentFloor == floor) return;
      this.currentFloor < floor
        ? (this.direction = true)
        : (this.direction = false);
      this.floorToGo = floor;
      this.isOpen = false;
      let duration = Math.abs(floor - this.currentFloor) * 1000 + 1;
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
          this.floorToGo = 0;
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
    cellData() {
      return { currentFloor: this.currentFloor, floorToGo: this.floorToGo };
    },
  },

  watch: {
    currentFloor() {
      this.$emit("save", this.cellData);
    },
    floorToGo() {
      this.$emit("save", this.cellData);
    },
  },
};
</script>

<style scoped lang="scss">
.indicator {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  background: rgb(104, 104, 104);
  position: absolute;
  top: 10%;
  left: 20%;
  width: 60%;
  height: 20%;
  border-radius: 5px;
  & div {
    color: white;
  }
}
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
