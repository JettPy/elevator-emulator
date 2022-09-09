<template>
  <div class="button-bar" :style="style_bar">
    <Floor
      v-for="i in this.floorCount" :key="i - 1"
      v-bind:floorNumber="i"
      v-bind:handleButtonClick="this.handleButtonClick"
      v-bind:isClicked="this.isClicked[i - 1]"
    />
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
    handleButtonClick: Function,
    isClicked: Array
  },
  computed: {
    style_bar() {
      return {
        "height": `${120 * this.floorCount}px`
      }
    }
  },
}
</script>

<style scoped>
.button-bar {
  margin: auto 20px;
  margin-top: max(auto, 20px);
  margin-bottom: max(auto, 20px);
  width: 150px;
  min-width: 20px;
  display: flex;
  flex-direction: column-reverse;
  background-color: #3d92fa;
}

@media screen and (max-width: 800px) {
  .button-bar {
    margin-left: 0px;
    margin-right: 10px;
  }
}
</style>
