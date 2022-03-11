<template>
  <div>
    <FloorDivider :floors="btnAmount" />
    <div class="flex">
      <div v-if="render" class="h-screen flex">
        <ElevatorCell
          v-for="index in elevatorAmount"
          :key="index"
          @open="opened"
          @save="saveData"
          ref="cells"
          :cellInfo="cellInfo[index - 1]"
        />
      </div>
      <div class="left-4 ml-4 h-screen flex flex-col-reverse justify-between">
        <ElevatorButton
          v-for="index in btnAmount"
          :key="index"
          :floor="index"
          :active="chosenFloors.includes(index) || queue.includes(index)"
          @called="called"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ElevatorButton from "./components/ElevatorButton.vue";
import FloorDivider from "./components/FloorDivider.vue";
import ElevatorCell from "./components/ElevatorCell.vue";
import { nextTick } from "@vue/runtime-core";
export default {
  components: {
    ElevatorButton,
    FloorDivider,
    ElevatorCell,
  },

  methods: {
    called(v) {
      if (this.queue.includes(v)) return;
      this.queue.push(v);
    },
    opened() {
      this.saveData();
      if (
        this.queue.length != 0 &&
        this.$refs.cells.some((cell) => cell.isOpen)
      )
        this.callElevator();
    },
    saveData() {
      nextTick(() => {
        this.cellInfo = [];
        this.$refs.cells.forEach((cell) => {
          this.cellInfo.push({
            currentFloor: cell.currentFloor,
            floorToGo: cell.floorToGo,
          });
        });
        localStorage.setItem("data", JSON.stringify(this.cellInfo));
        localStorage.setItem("queue", JSON.stringify(this.queue));
        this.cellInfo = JSON.parse(localStorage.getItem("data"));
      });
    },
    callElevator() {
      let floor = this.queue[0];
      var closest = this.$refs.cells.reduce((prev, curr) => {
        if (!prev.isOpen) return curr;
        if (!curr.isOpen) return prev;
        if (
          Math.abs(curr.currentFloor - floor) <
          Math.abs(prev.currentFloor - floor)
        )
          return curr;
        return prev;
      });
      closest.moveToFloor(floor);
      this.queue = this.queue.slice(1);
    },
  },

  mounted() {
    nextTick(() => {
      this.btnAmount = +process.env.VUE_APP_BTN_AMOUNT;
      this.elevatorAmount = +process.env.VUE_APP_ELEVATOR_AMOUNT;
      if (localStorage.getItem("data")) {
        this.cellInfo = JSON.parse(localStorage.getItem("data"));
      }
      if (localStorage.getItem("queue")) {
        this.queue = JSON.parse(localStorage.getItem("queue"));
      }
      this.render = true;

      this.saveData();
    });
  },

  data() {
    return {
      btnAmount: 1,
      elevatorAmount: 1,
      queue: [],
      cellInfo: [],
      render: false,
    };
  },
  watch: {
    queueCopy() {
      this.saveData();
      if (this.queue.length != 0 && this.cellInfo.some((c) => c.floorToGo == 0))
        this.callElevator();
    },
  },

  computed: {
    chosenFloors() {
      return this.cellInfo.map((cell) => cell.floorToGo);
    },
    queueCopy() {
      return this.queue.slice();
    },
  },
};
</script>

<style></style>
