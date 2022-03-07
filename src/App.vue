<template>
  <div>
    <FloorDivider :floors="btnAmount" />
    <div class="flex">
      <div class="h-screen">
        <ElevatorCell
          @open="opened"
          @save="saveData"
          ref="cell"
          :startingFloor="cellInfo.currentFloor"
          :finalFloor="cellInfo.floorToGo"
        />
      </div>
      <div class="left-4 ml-4 h-screen flex flex-col-reverse justify-between">
        <ElevatorButton
          v-for="index in btnAmount"
          :key="index"
          :floor="index"
          :active="queue.includes(index)"
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
      this.$refs.cell.isOpen && this.$refs.cell.moveToFloor(this.queue[0]);

      console.log(this.queue);
    },
    opened() {
      this.queue = this.queue.slice(1);
      if (this.queue.length == 0) return;
      this.$refs.cell.moveToFloor(this.queue[0]);
    },
    saveData(data) {
      localStorage.setItem("data", JSON.stringify(data));
      localStorage.setItem("queue", JSON.stringify(this.queue));
      this.cellInfo = data;
    },
  },

  mounted() {
    this.btnAmount = +process.env.VUE_APP_BTN_AMOUNT;
    this.cellInfo = JSON.parse(localStorage.getItem("data"));
    this.queue = JSON.parse(localStorage.getItem("queue"));
    if (this.queue.length != 0) this.$refs.cell.moveToFloor(this.queue[0]);
  },

  data() {
    return {
      btnAmount: 1,
      queue: [],
      cellInfo: {},
    };
  },
};
</script>

<style></style>
