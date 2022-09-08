<template>
  <div class="floor">
    <p class="floor__caption">{{ this.floorNumber }}</p>
    <button v-if="isClicked" disabled class="floor__button floor__button_clicked" @click="clickedFloor"></button>
    <button v-else class="floor__button" @click="clickedFloor"></button>
  </div>
</template>

<script>

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Floor",
  props: {
    floorNumber: Number,
    addFloor: Function,
    checkLift: Function
  },
  data() {
    return {
      isClicked: false
    }
  },
  methods: {
    clickedFloor() {
      if (!this.checkLift(this.floorNumber)) {
        this.isClicked = true;
        this.addFloor(this.floorNumber);
      }
    }
  }
}
</script>

<style>
.floor {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-bottom: solid black 1px;
  box-sizing: border-box;
}

.floor__caption {
  font-size: 20px;
  font-weight: 900;
  color: maroon;
  margin-bottom: 40px;
  font-family: Arial, Helvetica, sans-serif;
}

.floor__button {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background-color: lawngreen;
  cursor: pointer;
}

.floor__button_clicked {
  background-color: crimson;
  
}
</style>
