<template>
  <Elevator v-for="i in this.elevatorsCount" :key="i" />
  <ButtonBar  v-bind:addFloor="this.addFloorToQueue" />
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
      queue: []
    };
  },
  methods: {
    addFloorToQueue(floorNumber) {
      this.queue.push(floorNumber);
      console.log(this.queue);
    },
    deliteFloorFromQueue() {
      this.queue.shift();
      console.log(this.queue);
    },
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
