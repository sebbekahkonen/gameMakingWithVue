<template>
  <div class="gameOver">
    <img class="gameOverImage" src="../assets/game-over.png" />
    <button @click="onclickNewGame()">NEW GAME</button>
    <button></button>
  </div>
</template>

<script>
export default {
  methods: {
    onclickNewGame() {
      console.log("test");
      this.$emit("start-game");
    },
  },
};
</script>

<style>
.gameOver {
  background-color: transparent;
  width: 1000px;
  height: 1000px;
  position: absolute;
  top: 0%;
  width: 100vw;
  height: 104vw;
}
.gameOverImage {
  /* background: url("../assets/game-over.png"); */
  /* background-color: green; */
  /* background-size: 100%;
  background-repeat: no-repeat;
  width: 600px;
  height: 600px;
  top: 30%;
  left: 30%;
  position: absolute; */
}
</style>