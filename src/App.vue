<template>
  <div>
    <FloorDivider :floors="btnAmount" />
    <div class="flex">
      <div class="h-screen">
        <ElevatorCell />
      </div>
      <div class="left-4 ml-4 h-screen flex flex-col-reverse justify-between">
        <ElevatorButton
          v-for="index in btnAmount"
          :key="index"
          :floor="index"
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
      console.log(v);
    },
  },

  mounted() {
    this.btnAmount = +process.env.VUE_APP_BTN_AMOUNT;
  },

  data() {
    return {
      btnAmount: 1,
    };
  },
};
</script>

<style></style>
