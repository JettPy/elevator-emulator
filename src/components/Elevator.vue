<template>
  <div class="shaft" :style="style_shaft">
    <div class="lift" 
      :style="style_lift"
      :class="{lift_resting: this.isResting}"
    >
    <p class="lift__display">{{ message }}</p>
    </div>
  </div>
</template>

<script>

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Elevator',
  props: {
    index: Number,
    floorCount: Number,
    handleElevatorArrival: Function,
    floorRequestQueue: Array,
    liftIndex: Number,
    setIsMoving: Function,
    isResting: Boolean,
    setIsResting: Function,
    handleGetFist: Function,
    setLiftPosition: Function,
    currPosition: Number,
    toggler: Boolean,
    handleElevatorMoving: Function,
    targetFloor: Number,
    setTargetFloor: Function,
    saveTargets: Function
  },
  data() {
    return {
      message: '',
      time: 0,
      checker: null,
    }
  },
  created() {
    if (this.targetFloor) {
      this.elevatorMove(this.targetFloor)
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
      this.setIsResting(true, this.index);
      this.message = 'waiting';
      setTimeout(()=>{
        this.setIsResting(false, this.index);
        this.message = '';
        this.handleElevatorArrival(this.currPosition, this.index);
        this.setTargetFloor(null, this.index);
        this.saveTargets();
      }, 3000)
    },
    elevatorMove(targetFloor) {
      if (!targetFloor) {
        return;
      }
      if (this.targetFloor == null)
        this.setTargetFloor(this.handleGetFist(), this.index);
      this.saveTargets();
      this.handleElevatorMoving(this.index);
      let distance = targetFloor - this.currPosition;
      if (distance > 0) {
        this.message = targetFloor + ' up'
      } else {
        this.message = targetFloor + ' down'
      }
      this.time = Math.abs(distance);
      this.setLiftPosition(this.currPosition + distance, this.index);
      this.setIsMoving(true, this.index);
      setTimeout(() => {
        this.elevatorArrive();
        this.setIsMoving(false, this.index);
        this.setTargetFloor(null, this.index);
      }, this.time * 1000);
    }
  },
  watch: {
    toggler() {
      if (this.index == this.liftIndex) {
        this.setTargetFloor(this.floorRequestQueue[0], this.index);
        this.elevatorMove(this.floorRequestQueue[0]);
      }
    }
  }
}
</script>

<style scoped>
.shaft {
  margin: auto 0;
  margin-left: 20px;
  width: 200px;
  min-width: 30px;
  background-color: #02244e;
  border: solid black 1px;
  box-sizing: border-box;
  position: relative;
}

.lift {
  width: 100%;
  height: 120px;
  background-color: #0A74F5;
  position: absolute;
}

.lift_resting {
  animation: 1.5s infinite linear blink
}

.lift__display {
  margin: auto;
  width: 45%;
  height: 20px;
  font-size: 14px;
  font-weight: 600;
  font-family: Arial, Helvetica, sans-serif;
  text-align: center;
  color: #c01509;
  background-color:#aed1fc;
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
    background-color:#0af597;
  }
}

@media screen and (max-width: 800px) {
  .shaft {
    margin-left: 0px;
  }

  .shaft:first-child {
    margin-left: 10px;
  }
  
  .lift__display {
    font-size: 10px;
  }
}
</style>
