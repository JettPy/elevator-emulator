<template>
  <div class="shaft">
    <div class="lift"></div>
  </div>
</template>

<script>

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Elevator'
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
  display: grid;
  grid-template-rows: repeat(5, 1fr);
}

.lift {
  background-color: brown;
  grid-row-start: 5;
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
