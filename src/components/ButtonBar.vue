<template>
  <div class="button-bar">
    <Floor v-for="i in this.floorCount" :key="i - 1" v-bind:addFloor="this.addFloor" v-bind:floorNumber="this.floorCount - i + 1" v-bind:checkLift="this.checkLift" />
  </div>
</template>

<script>
import Floor from './Floor.vue';

export default {
    name: "ButtonBar",
    components: {
      Floor
    },
    props: {
      floorCount: Number,
      addFloor: Function,
      checkLift: Function
    }
}
</script>

<style>
.button-bar {
  height: calc(100% - 40px);
  width: 150px;
  min-width: 20px;
  margin: auto 20px;
  background-color: coral;
  display: grid;
}

@media screen and (max-width: 800px) {
  .button-bar {
    margin-left: 0px;
    margin-right: 10px;
  }
}
</style>
