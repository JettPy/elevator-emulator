<template>
  <div class="shaft">
    <div class="lift" :style="style"></div>
  </div>
</template>

<script>

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Elevator',
  props: {
    floorCount: Number,
    updatePosition: Function
  },
  data() {
    return {
      currPosition: 1,
      isResting: false
    }
  },
  computed: {
    style() {
      return {
        "height": `calc(100% / ${this.floorCount})`
      }
    }
  },
  methods: {

  }
}
</script>

<style>
.shaft {
  width: 200px;
  min-width: 30px;
  height: calc(100% - 40px);
  margin: auto 0;
  margin-left: 20px;
  background-color: beige;
  border: solid black 1px;
  box-sizing: border-box;
  position: relative;
}

.lift {
  width: 100%;
  background-color: brown;
  position: absolute;
  bottom: 0;
}

@media screen and (max-width: 800px) {

  .shaft {
    margin-left: 0px;
  }

  .shaft:first-child {
    margin-left: 10px;
  }

}
</style>
