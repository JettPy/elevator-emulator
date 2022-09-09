<template>
  <div class="shaft" :style="style_shaft">
    <div class="lift" 
      :style="style_lift"
      :class="{lift_resting: isResting}"
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
    floorCount: Number,
    handleElevatorArrival: Function,
    floorRequestQueue: Array
  },
  data() {
    return {
      message: '',
      currPosition: 1,
      isResting: false,
      isMoving: false,
      time: 0,
      isActive: false,
      checker: null
    }
  },
  created() {
    let lift = JSON.parse(localStorage.getItem("lift"));
    if (lift) {
      this.currPosition = lift;
    }
    let queue = JSON.parse(localStorage.getItem("queue"));
    if (queue) {
      if (queue.length) {
        this.isActive = true;
      }
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
      this.isResting = true;
      this.message = 'waiting';
      setTimeout(()=>{
        this.isResting = false;
        this.message = '';
        this.handleElevatorArrival(this.currPosition);
      }, 3000)
    },
    elevatorMove(targetFloor) {
      let distance = targetFloor - this.currPosition;
      if (distance > 0) {
        this.message = targetFloor + ' up'
      } else {
        this.message = targetFloor + ' down'
      }
      this.time = Math.abs(distance);
      this.currPosition += distance;
      this.isMoving = true;
      setTimeout(() => {
        this.elevatorArrive();
        this.isMoving = false;
      }, this.time * 1000);
    }
  },
  watch: {
    floorRequestQueue: {
      handler() {
        this.isActive = !(this.floorRequestQueue.length == 0);
      },
      deep: true
    },
    isActive(value) {
      if (value) {
        this.checker = setInterval(() => {
          if (this.isResting || this.isMoving)
            return;
          this.elevatorMove(this.floorRequestQueue[0]);
        }, 100);
      } else {
        clearInterval(this.checker)
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
