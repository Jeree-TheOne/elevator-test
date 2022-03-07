<template>
  <div>
    <FloorDivider :floors="btnAmount" />
    <div class="flex">
      <div class="h-screen flex">
        <ElevatorCell
          v-for="index in elevatorAmount"
          :key="index"
          @open="opened"
          @save="saveData"
          ref="cells"
          :startingFloor="cellInfo.currentFloor"
          :finalFloor="cellInfo.floorToGo"
        />
      </div>
      <div class="left-4 ml-4 h-screen flex flex-col-reverse justify-between">
        <ElevatorButton
          v-for="index in btnAmount"
          :key="index"
          :floor="index"
          :active="chosenFloors.includes(index)"
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
      if (this.cellInfo.currentFloor == v && this.cellInfo.floorToGo == 0)
        return;
      this.queue.push(v);
      this.callElevator();
    },
    opened() {
      this.saveData();
    },
    saveData() {
      this.cellInfo = [];
      this.$refs.cells.forEach((cell) => {
        this.cellInfo.push({
          currentFloor: cell.currentFloor,
          floorToGo: cell.floorToGo,
        });
      });
      localStorage.setItem("data", JSON.stringify(this.cellInfo));
      localStorage.setItem("queue", JSON.stringify(this.queue));
    },
    callElevator() {
      var closest = this.cellInfo.reduce((prev, curr) => {
        if (curr.floorToGo != 0) return prev;
        // в браузере undefinded curr.currentFloor, в консоль выводит ._.
        Math.abs(curr.currentFloor - this.queue[0]) <
        Math.abs(prev.currentFloor - this.queue[0])
          ? curr
          : prev;
      });

      this.$refs.cells[this.cellInfo.indexOf(closest)].moveToFloor(
        this.queue[0]
      );
      this.queue = this.queue.slice(1);
    },
  },

  mounted() {
    nextTick(() => {
      this.btnAmount = +process.env.VUE_APP_BTN_AMOUNT;
      this.elevatorAmount = +process.env.VUE_APP_ELEVATOR_AMOUNT;
      this.cellInfo = JSON.parse(localStorage.getItem("data"));
      this.queue = JSON.parse(localStorage.getItem("queue"));
      while (this.queue.length != 0) this.callElevator();
    });
  },

  data() {
    return {
      btnAmount: 1,
      elevatorAmount: 1,
      queue: [],
      cellInfo: [],
    };
  },
  watch: {
    queue: {
      handler: function () {
        this.saveData();
      },
    },
  },
  computed: {
    chosenFloors() {
      return this.cellInfo.map((cell) => cell.floorToGo);
    },
  },
};
</script>

<style></style>
