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
    async elevatorArrive() {
      this.isResting = true;
      this.message = '';
      await setTimeout(()=>{
        this.isResting = false;
        this.handleElevatorArrival(this.currPosition);
      }, 3000)
    },
    async elevatorMove(targetFloor) {
      let distance = targetFloor - this.currPosition;
      if (distance > 0) {
        this.message = targetFloor + ' up'
      } else {
        this.message = targetFloor + ' down'
      }
      this.time = Math.abs(distance);
      this.currPosition += distance;
      await setTimeout(() => {
        this.elevatorArrive();
        console.log(`Выполнено за ${this.time * 1000}`)
      }, this.time * 1000);
    }, 
    async checkIsResting() {
      return this.isResting;
    }
  },
  watch: {
    floorRequestQueue: {
      async handler() {
        console.log(this.floorRequestQueue)
        if (this.floorRequestQueue.length == 0)
          return
        let state = await this.checkIsResting();
        if (!state) {
          await this.elevatorMove(this.floorRequestQueue[0]);
        }
      },
      deep: true
    }
  }
}
</script>

<style scoped>
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

.lift__display {
  font-size: 14px;
  font-weight: 600;
  font-family: Arial, Helvetica, sans-serif;
  margin: auto;
  background-color:aqua;
  text-align: center;
  width: 45%;
  height: 20px;
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
  
  .lift__display {
    font-size: 10px;
  }

}
</style>
