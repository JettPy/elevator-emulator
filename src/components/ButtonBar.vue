<template>
  <div class="button-bar">
    <div class="floor">
      <button class="floor-button" @click="clickedFloor(5)">5</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clickedFloor(4)">4</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clickedFloor(3)">3</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clickedFloor(2)">2</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clickedFloor(1)">1</button>
    </div>
  </div>
</template>

<script>

export default {
  name: "ButtonBar",
  props: {
    addFloor: Function
  },
  data() {
    return {
      a: 1
    };
  },
  methods: {
    clickedFloor(floorNumber) {
      this.addFloor(floorNumber);
    }
  }
}
</script>

<style>
.button-bar {
  height: calc(100% - 40px);
  width: 100px;
  min-width: 20px;
  margin: auto 20px;
  background-color: coral;
  display: grid;
}

.floor {
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom: solid black 1px;
  box-sizing: border-box;
}

.floor-button {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background-color: cadetblue;
  cursor: pointer;
}

@media screen and (max-width: 800px) {

  .button-bar {
    margin-left: 0px;
    margin-right: 10px;
  }

}
</style>
