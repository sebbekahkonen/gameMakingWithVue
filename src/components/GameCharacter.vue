<template>
  <div class="mainDiv" :class="getGameOn">
    <div class="parentDiv">
      <input
        ref="inputField"
        type="text"
        @keydown.left="moveLeft()"
        @keydown.right="moveRight()"
        @keypress="checkKeyPress"
        v-model="leftStyle"
      />
    </div>
    <div
      ref="character"
      :style="{ left: `${leftStyle}%` }"
      class="character"
    ></div>
    <div ref="redCar" class="redCar" :class="getDifficulty"></div>
    <div ref="blueCar" class="blueCar" :class="getDifficulty"></div>
    <div ref="greenCar" class="greenCar" :class="getDifficulty"></div>
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
    leftStyle: 31,
    isGameOver: false,
    countDown: 3,
    gameIsOn: false,
    interval: null,
    difficultyTimer: 0,
    difficultyNormal: false,
    difficultyMedium: false,
    difficultyHard: false,
    difficultyExpert: false,
  }),

  computed: {
    getGameOn() {
      return {
        gameOn: this.gameIsOn,
      };
    },
    getDifficulty() {
      return {
        normal: this.difficultyNormal,
        medium: this.difficultyMedium,
        hard: this.difficultyHard,
        expert: this.difficultyExpert,
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
      console.log(this.leftStyle);
      // this.leftStyle = this.leftStyle - 5;
      if (this.leftStyle === 31) {
        return;
      } else {
        this.leftStyle = this.leftStyle - 5;
      }
      // if(this.leftStyle === 21)
      // if (this.leftStyle === 60) {
      //   this.leftStyle = this.leftStyle - 29;
      // } else {
      //   return;
      // }
    },

    moveRight() {
      // console.log(this.leftStyle);
      if (this.leftStyle === 61) {
        return;
      } else {
        this.leftStyle = this.leftStyle + 5;
      }
      //   this.leftStyle = this.leftStyle + 29;
      // } else {
      //   return;
      // }
    },

    checkKeyPress(event) {
      event.preventDefault();
    },

    stopGame() {
      this.gameIsOn = false;
      this.isGameOver = true;
      clearInterval(this.interval);
      clearInterval(this.difficultyTimer);
    },

    startGame() {
      this.difficultyTimer = setInterval(() => {
        this.difficultyTimer++;
        if (this.difficultyTimer === 10) {
          console.log("wow you've reached 10");
        }
      }, 1000);
      if (this.difficultyTimer === 10) {
        console.log("timer is 10");
      }
      this.$refs.inputField.focus();
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
          (positionLeftCharacter === 253 || positionLeftCharacter === 392) &&
          positionRedCar > 400 &&
          positionRedCar < 470
        ) {
          this.stopGame();
          return;
        }

        if (
          (positionLeftCharacter === 490 || positionLeftCharacter === 758) &&
          positionBlueCar > 400 &&
          positionBlueCar < 470
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
.mainDiv {
  width: 100%;
  height: 100%;
}

button {
  width: 200px;
  height: 50px;
}

.character {
  position: absolute;
  background: url("../assets/character.png");
  background-size: 100%;
  background-repeat: no-repeat;
  width: 10%;
  height: 10%;
  top: 50%;
}

.parentDiv {
  position: relative;
  width: 100%;
  height: 100%;
}

input {
  /* width: 100vw; */
  width: 100%;
  height: 120vw;
  border: 0;
  /* position: relative; */
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

/* difficulty: NORMAL */
.gameOn .normal .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 3.5s;
  -webkit-animation-name: car-vertical-normal;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.gameOn .normal .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 3.5s;
  -webkit-animation-name: car-vertical-normal;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}

.gameOn .normal .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 3.5s;
  -webkit-animation-name: car-vertical-normal;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 5s;
}

@keyframes car-vertical-normal {
  from {
    top: -60%;
  }
  to {
    top: 120%;
  }
}

/* difficulty: MEDIUM */

.gameOn .medium .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 2.5s;
  -webkit-animation-name: car-vertical-medium;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.gameOn .medium .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 2.5s;
  -webkit-animation-name: car-vertical-medium;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}

.gameOn .medium .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 2.5s;
  -webkit-animation-name: car-vertical-medium;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 5s;
}

@keyframes car-vertical-medium {
  from {
    top: -60%;
  }
  to {
    top: 120%;
  }
}

/* difficulty: HARD */
.gameOn .hard .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car-vertical-hard;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.gameOn .hard .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car-vertical-hard;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}

.gameOn .hard .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car-vertical-hard;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 5s;
}

@keyframes car-vertical-hard {
  from {
    top: -60%;
  }
  to {
    top: 120%;
  }
}

/* difficulty: EXPERT */
.gameOn .expert .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 1s;
  -webkit-animation-name: car-vertical-expert;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.gameOn .expert .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 1s;
  -webkit-animation-name: car-vertical-expert;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}

.gameOn .expert .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 1s;
  -webkit-animation-name: car-vertical-expert;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 5s;
}

@keyframes car-vertical-expert {
  from {
    top: -60%;
  }
  to {
    top: 120%;
  }
}
</style>