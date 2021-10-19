<template>
  <div :class="getGameOn">
    <input
      ref="inputField"
      type="text"
      @keydown.left="moveLeft()"
      @keydown.right="moveRight()"
      @keypress="checkKeyPress"
      v-model="leftStyle"
    />
    <div
      ref="character"
      :style="{ left: `${leftStyle}%` }"
      class="character"
    ></div>
    <div ref="redCar" class="redCar"></div>
    <div ref="blueCar" class="blueCar"></div>
    <GameOver v-if="isGameOver" @start-game="startCountDown()" />
    <countDown v-if="countDown != 0" :count-down-number="countDown" />
  </div>
</template>


<script>
// import { ref, onMounted } from "vue";
import GameOver from "./GameOver.vue";
import countDown from "./countDown.vue";
export default {
  components: {
    GameOver,
    countDown,
  },

  data: () => ({
    leftStyle: 35,
    isGameOver: false,
    countDown: 3,
    gameIsOn: false,
    interval: null,
  }),

  computed: {
    getGameOn() {
      return {
        gameOn: this.gameIsOn,
      };
    },
  },

  created() {
    this.startCountDown();
  },

  methods: {
    startCountDown() {
      this.countDown = 3;
      const interval = setInterval(() => {
        console.log(this.countDown);
        this.countDown--;
        if (this.countDown === 0) {
          clearInterval(interval);
          this.gameIsOn = true;
          this.startGame();
        }
      }, 1000);
    },

    moveLeft() {
      if (this.leftStyle === 60) {
        this.leftStyle = this.leftStyle - 25;
      } else {
        return;
      }
    },

    moveRight() {
      if (this.leftStyle === 35) {
        this.leftStyle = this.leftStyle + 25;
      } else {
        return;
      }
    },

    checkKeyPress(event) {
      event.preventDefault();
    },

    stopGame() {
      this.gameIsOn = false;
      this.isGameOver = true;
      clearInterval(this.interval);
    },

    startGame() {
      this.gameIsOn = true;
      this.isGameOver = false;
      this.interval = setInterval(() => {
        let positionRedCar = Math.round(
          this.$refs.redCar.getBoundingClientRect().top
        );

        let positionBlueCar = Math.round(
          this.$refs.blueCar.getBoundingClientRect().top
        );

        let positionLeftCharacter = Math.round(
          this.$refs.character.getBoundingClientRect().left
        );

        if (
          positionLeftCharacter == 476 &&
          positionRedCar > 840 &&
          positionRedCar < 870
        ) {
          this.stopGame();
          return;
        }

        if (
          positionLeftCharacter == 817 &&
          positionBlueCar > 840 &&
          positionBlueCar < 870
        ) {
          this.stopGame();
          return;
        }
      }, 10);
    },
  },
};
</script>

<style scoped>
button {
  width: 200px;
  height: 50px;
}

.character {
  position: absolute;
  background: url("../assets/character.png");
  background-size: 100%;
  background-repeat: no-repeat;
  width: 80px;
  height: 100px;
  top: 60%;
}

input {
  width: 100vw;
  height: 100vw;
  border: 0;
  position: relative;
  left: 0%;
  background: url("../assets/road.png");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  object-fit: cover;
  align-content: center;
  text-align: center;
  color: white;
  padding: 0;
}

.gameOn .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 60%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car2-vertical;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: alternate-reverse;
}
.gameOn .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 35%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car1-vertical;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: alternate-reverse;
}

@keyframes car1-vertical {
  from {
    top: 10%;
  }
  to {
    top: 80%;
  }
}

@keyframes car2-vertical {
  from {
    top: 80%;
  }
  to {
    top: 10%;
  }
}
</style>