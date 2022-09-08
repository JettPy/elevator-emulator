<template>
  <div class="shaft">
    <div class="lift"></div>
  </div>
  <div class="shaft">
    <div class="lift"></div>
  </div>
  <div class="shaft">
    <div class="lift"></div>
  </div>
  <div class="shaft">
    <div class="lift"></div>
  </div>
  <div class="shaft">
    <div class="lift"></div>
  </div>
  <div class="button-bar">
    <div class="floor">
      <button class="floor-button" @click="clicked5">5</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clicked4">4</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clicked3">3</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clicked2">2</button>
    </div>
    <div class="floor">
      <button class="floor-button" @click="clicked1">1</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  methods: {
    clicked1() {
      console.log('Вызов 1 этаж');
    },
    clicked2() {
      console.log('Вызов 2 этаж');
    },
    clicked3() {
      console.log('Вызов 3 этаж');
    },
    clicked4() {
      console.log('Вызов 4 этаж');
    },
    clicked5() {
      console.log('Вызов 5 этаж');
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  border: 0;
}

#app {
  height: 100vh;
  display: flex;
  background-color: burlywood;
}

.shaft {
  width: 200px;
  min-width: 30px;
  height: calc(100% - 40px);
  margin: auto 0;
  margin-left: 20px;
  background-color: beige;
  border: solid black 1px;
  box-sizing: border-box;
  display: grid;
  grid-template-rows: repeat(5, 1fr);
}

.lift {
  background-color: brown;
  grid-row-start: 5;
}

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

  .shaft {
    margin-left: 0px;
  }

  .shaft:first-child {
    margin-left: 10px;
  }

  .button-bar {
    margin-left: 0px;
    margin-right: 10px;
  }

}
</style>
