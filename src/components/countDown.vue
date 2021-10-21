<template>
  <div class="countDownDiv">
    <h1>{{ countDownNumber }}</h1>
  </div>
</template>

<script>
export default {
  props: {
    countDownNumber: {
      type: Number,
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.countDownDiv {
  top: -10%;
  margin-left: auto;
  margin-right: auto;
  left: 25%;
  width: 50%;
  position: absolute;
  /* width: 300px; */
  height: 600px;
  /* background-color: green; */
  text-align: center;
}
.countDownDiv h1 {
  top: 0%;
  left: 25%;
  width: 50%;
  margin-left: auto;
  margin-right: auto;
  font-family: "Press Start 2P", cursive;
  font-size: 300px;
  color: #000;
  position: absolute;
}
</style>