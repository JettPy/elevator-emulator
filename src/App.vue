<template>
  <Elevator
    v-bind:floorCount="this.floorCount"
    v-bind:handleElevatorArrival="this.handleElevatorArrival"
    v-bind:targetFloor="this.targetFloor"
  />
  <ButtonBar
    v-bind:floorCount="this.floorCount"
    v-bind:handleButtonClick="this.handleButtonClick"
    v-bind:isClicked="this.isClicked"
  />
</template>

<script>
import Elevator from './components/Elevator.vue'
import ButtonBar from './components/ButtonBar.vue'

export default {
  name: "App",
  components: {
    Elevator,
    ButtonBar
  },
  data() {
    return {
      floorCount: 7,
      floorRequestQueue: [],
      liftPosition: 1,
      isClicked: [],
      targetFloor: null
    };
  },
  created() {
    for (let i = 0; i < this.floorCount; i++) {
      this.isClicked.push(false);
    }
  },
  methods: {
    handleButtonClick(floorNumber) {
      if (this.isClicked[floorNumber - 1] || this.liftPosition == floorNumber) {
        return;
      }
      this.isClicked[floorNumber - 1] = true;
      this.floorRequestQueue.push(floorNumber);
    },
    handleElevatorArrival(floor) {
      this.liftPosition = floor;
      this.isClicked[this.liftPosition - 1] = false;
      //запуск анимации отдыха + обновление статуса лифта
    },
    handleElevatorMove(targetFloor) {
      this.targetFloor = targetFloor;
      console.log(targetFloor)
      setTimeout(this.handleElevatorArrival, 3000, targetFloor);
    }
  },
  watch: {
    floorRequestQueue: {
      handler() {
        if (this.floorRequestQueue.length > 0)
          this.handleElevatorMove(this.floorRequestQueue.shift());
      },
      deep:true
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  border: 0;
}

#app {
  padding-top: 20px;
  padding-bottom: 20px;
  display: flex;
  background-color: burlywood;
}
</style>
