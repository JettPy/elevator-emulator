<template>
  <div class="shaft" :style="style_shaft">
    <div class="lift" 
      :style="style_lift"
      :class="{lift_resting: isResting}"
    >
    </div>
  </div>
</template>

<script>

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Elevator',
  props: {
    floorCount: Number,
    handleElevatorArrival: Function,
    targetFloor: Number
  },
  data() {
    return {
      currPosition: 1,
      isResting: false,
      time: 0
    }
  },
  computed: {
    style_lift() {
      return {
        "bottom": `${(this.currPosition - 1) * 120}px`,
        "transition": `bottom ${this.time}s linear`
      }
    },
    style_shaft() {
      return {
        "height": `${120 * this.floorCount}px`
      }
    }
  },
  methods: {
    elevatorArrive() {
      this.handleElevatorArrival();
      this.isResting = true;
      setTimeout(()=>{
        this.isResting = false;
      }, 3000)
    },
    elevatorMove(targetFloor) {
      let distance = targetFloor - this.currPosition;
      this.time = Math.abs(distance);
      this.currPosition += distance;
      setTimeout(() => {
        this.elevatorArrive();
        console.log(`Выполнено за ${this.time * 1000}`)
      }, this.time * 1000);

    }
  },
  watch: {
    targetFloor: {
      handler() {
        this.elevatorMove(this.targetFloor);
      }
    }
  }
}
</script>

<style>
.shaft {
  width: 200px;
  min-width: 30px;
  margin: auto 0;
  margin-left: 20px;
  background-color: beige;
  border: solid black 1px;
  box-sizing: border-box;
  position: relative;
}

.lift {
  width: 100%;
  height: 120px;
  background-color: brown;
  position: absolute;
}

.lift_resting {
  animation-name: blink;
  animation-timing-function: linear;
  animation-duration: 1.5s;
  animation-iteration-count: infinite;
}

@keyframes blink {
  0%,
  50%,
  100% {
    opacity: 1;
  }

  25%,
  75% {
    opacity: 0.75;
  }
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
