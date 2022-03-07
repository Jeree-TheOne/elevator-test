<template>
  <div>
    <FloorDivider :floors="btnAmount" />
    <div class="flex">
      <div class="h-screen">
        <ElevatorCell @open="opened" ref="cell" />
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
      this.queue.push(v);
      this.$refs.cell.isOpen && this.$refs.cell.moveToFloor(this.queue[0]);

      console.log(this.queue);
    },
    opened(v) {
      console.log(v);
      this.queue = this.queue.slice(1);
      if (this.queue.length == 0) return;
      this.$refs.cell.moveToFloor(this.queue[0]);
    },
  },

  mounted() {
    this.btnAmount = +process.env.VUE_APP_BTN_AMOUNT;
  },

  data() {
    return {
      btnAmount: 1,
      queue: [],
    };
  },
};
</script>

<style></style>
