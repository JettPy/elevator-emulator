<template>
  <Elevator v-for="i in this.elevatorsCount" :key="i - 1" v-bind:floorCount="this.floorCount" v-bind:updatePosition="this.updatePosition"/>
  <ButtonBar v-bind:floorCount="this.floorCount" v-bind:addFloor="this.addFloorToQueue" v-bind:checkLift="this.checkLiftOnFloor"/>
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
      elevatorsCount: 5,
      floorCount: 5,
      queue: [],
      queueLen: 0,
      positions: []
    };
  },
  created() {
    for (let i = 0; i < this.elevatorsCount; i++) {
      this.positions.push(1);
    }
    console.log(this.positions);
  },
  methods: {
    addFloorToQueue(floorNumber) {
      this.queue.push(floorNumber);
      this.queueLen += 1;
      console.log(this.queue);
    },
    deliteFloorFromQueue() {
      this.queue.shift();
      this.queueLen -= 1;
      console.log(this.queue);
    },
    updatePosition(lift, floor) {
      this.positions[lift] = floor;
    },
    checkLiftOnFloor(floorNumber) {
      return this.positions.some((_, i) => floorNumber == this.positions[i])
    }
  },
  watch: {
    queueLen(value) {
      console.log(`Effect ${value}`);
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
  height: 100vh;
  display: flex;
  background-color: burlywood;
}
</style>
