<template>
  <ElevatorsBlock
    v-bind:elevatorCount="this.elevatorCount"
    v-bind:floorCount="this.floorCount"
    v-bind:handleElevatorArrival="this.handleElevatorArrival"
    v-bind:floorRequestQueue="this.floorRequestQueue"
    v-bind:handleGetFist="this.handleGetFist"
    v-bind:handleElevatorMoving="this.handleElevatorMoving"
  />
  <ButtonBar
    v-bind:floorCount="this.floorCount"
    v-bind:handleButtonClick="this.handleButtonClick"
    v-bind:isClicked="this.isClicked"
  />
</template>

<script>
import ElevatorsBlock from './components/ElevatorsBlock.vue';
import ButtonBar from './components/ButtonBar.vue';

export default {
  name: "App",
  components: {
    ElevatorsBlock,
    ButtonBar
},
  data() {
    return {
      elevatorCount: 3,
      floorCount: 7,
      floorRequestQueue: [],
      liftPosition: [],
      liftSavedPosition: [],
      isClicked: []
    };
  },
  created() {
    let lifts = JSON.parse(localStorage.getItem("lifts"));
    if (lifts) {
      this.liftPosition = lifts;
      this.liftSavedPosition = lifts.slice();
    } else {
      for (let i = 0; i < this.elevatorCount; i++) {
        this.liftPosition.push(1);
        this.liftSavedPosition.push(1);
      }
    }
    let queue = JSON.parse(localStorage.getItem("queue"));
    if (queue) {
      this.floorRequestQueue = queue;
    }
    let clicks = JSON.parse(localStorage.getItem("clicks"));
    if (clicks) {
      this.isClicked = clicks;
    } else {
      for (let i = 0; i < this.floorCount; i++) {
        this.isClicked.push(false);
      }
    }
    if (queue.length == 0) {
      this.isClicked = [];
      for (let i = 0; i < this.floorCount; i++) {
        this.isClicked.push(false);
      }
    }
  },
  methods: {
    handleGetFist() {
      return this.floorRequestQueue.shift();
    },
    handleButtonClick(floorNumber) {
      if (this.isClicked[floorNumber - 1] || this.liftPosition.some((liftPosition) => liftPosition == floorNumber)) {
        return;
      }
      this.isClicked[floorNumber - 1] = true;
      this.floorRequestQueue.push(floorNumber);
      this.saveData();
    },
    handleElevatorMoving(liftIndex) {
      this.liftPosition[liftIndex] = 0;
    },
    handleElevatorArrival(floor, liftIndex) {
      this.liftPosition[liftIndex] = floor;
      this.liftSavedPosition[liftIndex] = floor;
      this.isClicked[floor - 1] = false;
      this.saveData();
    },
    saveData() {
      localStorage.setItem("lifts", JSON.stringify(this.liftSavedPosition));
      localStorage.setItem("queue", JSON.stringify(this.floorRequestQueue));
      localStorage.setItem("clicks", JSON.stringify(this.isClicked));
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
  min-height: calc(100vh);
  display: flex;
  background-color: #92c3ff;
  box-sizing:border-box;
}
</style>
