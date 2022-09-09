<template>
  <Elevator
    v-bind:floorCount="this.floorCount"
    v-bind:handleElevatorArrival="this.handleElevatorArrival"
    v-bind:floorRequestQueue="this.floorRequestQueue"
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
      isClicked: []
    };
  },
  created() {
    let lift = JSON.parse(localStorage.getItem("lift"));
    if (lift) {
      this.liftPosition = lift;
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
  },
  methods: {
    handleButtonClick(floorNumber) {
      if (this.isClicked[floorNumber - 1] || this.liftPosition == floorNumber) {
        return;
      }
      this.liftPosition = 0;
      this.isClicked[floorNumber - 1] = true;
      this.floorRequestQueue.push(floorNumber);
      this.saveData();
    },
    handleElevatorArrival(floor) {
      this.liftPosition = floor;
      this.isClicked[this.liftPosition - 1] = false;
      this.floorRequestQueue.shift();
      this.saveData();
    },
    saveData() {
      localStorage.setItem("lift", JSON.stringify(this.liftPosition));
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
