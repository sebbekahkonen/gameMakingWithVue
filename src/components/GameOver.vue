<template>
  <div class="gameOver">
    <img class="gameOverImage" src="../assets/game-over.png" />
    <button class="newGameBtn" @click="onclickNewGame()">NEW GAME</button>
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
  align-content: center;
  background-color: transparent;
  width: 1000px;
  height: 120vw;
  position: absolute;
  top: 12%;
  width: 100vw;
  height: 104vw;
}
.gameOverImage {
  position: absolute;
  top: 10%;
  /* left: 5%; */
  /* width: 700px; */
  /* height: 700px; */
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
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
.newGameBtn {
  position: absolute;
  top: 80%;
  left: 43%;
}
</style>