<template>
  <Elevator v-for="i in this.elevatorCount" :key="i - 1"
    v-bind:index="i - 1" 
    v-bind:floorCount="this.floorCount"
    v-bind:handleElevatorArrival="this.handleElevatorArrival"
    v-bind:floorRequestQueue="this.floorRequestQueue"
    v-bind:liftIndex="this.liftIndex"
    v-bind:isResting="this.isResting[i - 1]"
    v-bind:setIsResting="this.setIsResting"
    v-bind:setIsMoving="this.setIsMoving"
    v-bind:handleGetFist="this.handleGetFist"
    v-bind:setLiftPosition="this.setLiftPosition"
    v-bind:currPosition="this.liftPosition[i - 1]"
    v-bind:toggler="this.toggler"
    v-bind:handleElevatorMoving="this.handleElevatorMoving"
    v-bind:targetFloor="this.targetFloor[i - 1]"
    v-bind:setTargetFloor="this.setTargetFloor"
    v-bind:saveTargets="this.saveTargets"
  />
</template>

<script>
import Elevator from './Elevator.vue'

export default {
  name: "ElevatorsBlock",
  components: {
    Elevator,
  },
  props: {
    elevatorCount: Number,
    floorCount: Number,
    handleElevatorArrival: Function,
    floorRequestQueue: Array,
    handleGetFist: Function,
    handleElevatorMoving: Function
  },
  data() {
    return {
      isResting: [],
      isMoving: [],
      liftPosition: [],
      isActive: false,
      checker: null,
      lastFloor: null,
      liftIndex: -1,
      toggler: true,
      targetFloor: []
    }
  },
  created() {
    let lifts = JSON.parse(localStorage.getItem("lifts"));
    if (lifts) {
      this.liftPosition = lifts;
    } else {
      for (let i = 0; i < this.elevatorCount; i++) {
        this.liftPosition.push(1);
      }
    }
    let queue = JSON.parse(localStorage.getItem("queue"));
    if (queue) {
      if (queue.length) {
        this.isActive = true;
      }
    }
    let targets = JSON.parse(localStorage.getItem("targets"));
    if (targets) {
      this.targetFloor = targets;
    }
    for (let i = 0; i < this.elevatorCount; i++) {
      this.isResting.push(false);
      this.isMoving.push(false);
    }
  },
  methods: {
    setLiftPosition(value, index) {
      this.liftPosition[index] = value;
    },
    setIsResting(value, index) {
      this.isResting[index] = value;
    },
    setIsMoving(value, index) {
      this.isMoving[index] = value;
    },
    findNearestFreeElevator(targetFloor) {
      let liftIndex = -1;
      let distance = this.floorCount;
      for (let i = 0; i < this.elevatorCount; i++) {
        if (!this.isResting[i] && !this.isMoving[i]) {
          if (Math.abs(this.liftPosition[i] - targetFloor) <= distance) {
            liftIndex = i;
            distance = Math.abs(this.liftPosition[i] - targetFloor)
          }
        }
      }
      return liftIndex;
    },
    checkIfStck() {
      return this.isMoving.some((value) => { value == true }) || this.isResting.some((value) => { value == true });
    },
    setTargetFloor(target, index) {
      this.targetFloor[index] = target;
    },
    saveTargets() {
      localStorage.setItem("targets", JSON.stringify(this.targetFloor));
    }
  },
  watch: {
    floorRequestQueue: {
      handler() {
        this.isActive = this.floorRequestQueue.length != 0;
      },
      deep: true
    },
    isActive(value) {
      if (value) {
        this.checker = setInterval(() => {
          if ((this.floorRequestQueue.length != 0 && this.lastFloor == this.floorRequestQueue[0]) && this.checkIfStck()) {
            this.isActive = this.floorRequestQueue.length != 0;
            return;
          }
          this.lastFloor = this.floorRequestQueue[0];
          let clock = setInterval(() => {
            this.liftIndex = this.findNearestFreeElevator(this.lastFloor);
            this.toggler = !this.toggler;
            if (this.liftIndex != -1) {
              clearInterval(clock);
            }
          }, 100);
        }, 100);
      } else {
        this.lastFloor = null;
        clearInterval(this.checker)
      }
    }
  }
}
</script>
