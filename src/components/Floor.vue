<template>
  <div class="floor">
    <p class="floor__caption">{{ this.floorNumber }}</p>
    <button class="floor__button"
      :class="{floor__button_clicked: isClicked}"
      @click="buttonClick">
    </button>
  </div>
</template>

<script>

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Floor",
  props: {
    floorNumber: Number,
    handleButtonClick: Function,
    isClicked: Boolean
  },
  methods: {
    buttonClick() {
      this.handleButtonClick(this.floorNumber);
    }
  }
}
</script>

<style scoped>
.floor {
  height: 100%;
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
