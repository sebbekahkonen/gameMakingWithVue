<template>
  <div class="winnerDiv">
    <img src="../assets/winner.png" />
    <h1>CONGRATS! <br />YOU HAVE WON</h1>
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

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");
.winnerDiv {
  top: 0%;
  left: 0%;
  position: absolute;
  width: 100%;
  height: 500px;
  font-family: "Press Start 2P", cursive;
  color: rgb(36, 31, 31);
  font-size: 30px;
}

.newGameBtn {
  padding: 2%;
  border-radius: 20px;
  width: 50%;
  margin-left: auto;
  margin-right: auto;
  left: 25%;
  color: white;
  background: linear-gradient(to top, rgb(54, 56, 56), rgb(224, 224, 224));
  font-family: "Press Start 2P", cursive;
  font-size: 30px;
}
</style>