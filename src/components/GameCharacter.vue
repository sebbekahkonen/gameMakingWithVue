<template>
  <div class="mainDiv" :class="[getDifficulty, getGameOn]">
    <div class="parentDiv">
      <input
        ref="inputField"
        type="text"
        @keydown.left="moveLeft()"
        @keydown.right="moveRight()"
        @keydown.up="moveUp()"
        @keydown.down="moveDown()"
        @keypress="checkKeyPress"
        v-model="leftStyle"
      />
    </div>
    <div
      ref="character"
      :style="{ left: `${leftStyle}%`, top: `${topStyle}%` }"
      class="character"
    ></div>
    <div ref="redCar" class="redCar"></div>
    <div ref="blueCar" class="blueCar"></div>
    <div ref="greenCar" class="greenCar"></div>
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
    topStyle: 50,
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
          this.difficultyMedium = false;
          this.difficultyHard = false;
          this.difficultyExpert = false;
          this.difficultyNormal = false;
          this.difficultyTimer = 0;
          this.startGame();
        }
      }, 1000);
    },

    moveLeft() {
      console.log(this.leftStyle);
      if (this.leftStyle === 31) {
        return;
      } else {
        this.leftStyle = this.leftStyle - 5;
      }
    },

    moveRight() {
      if (this.leftStyle === 61) {
        return;
      } else {
        this.leftStyle = this.leftStyle + 5;
      }
    },

    moveUp() {
      if (this.topStyle === 20) {
        return;
      } else {
        this.topStyle = this.topStyle - 5;
      }
    },

    moveDown() {
      if (this.topStyle === 80) {
        return;
      } else {
        this.topStyle = this.topStyle + 5;
      }
    },

    checkKeyPress(event) {
      event.preventDefault();
    },

    stopGame() {
      this.gameIsOn = false;
      this.difficultyNormal = false;
      this.difficultyMedium = false;
      this.difficultyHard = false;
      this.difficultyExpert = false;
      this.isGameOver = true;
      clearInterval(this.interval);
      clearInterval(this.difficultyTimer);
    },

    startGame() {
      const timerInterval = setInterval(() => {
        this.difficultyTimer++;
        /* difficulty: NORMAL*/
        if (this.difficultyTimer < 15) {
          this.difficultyNormal = true;
        }
        /* difficulty: MEDIUM*/
        if (this.difficultyTimer === 15) {
          console.log("MEDIUM level");
          this.difficultyNormal = false;
          this.difficultyMedium = true;
        }

        /* difficulty: HARD */
        if (this.difficultyTimer === 25) {
          console.log("HARD level");
          this.difficultyMedium = false;
          this.difficultyHard = true;
        }

        /* difficulty: EXPERT */
        if (this.difficultyTimer === 35) {
          console.log("EXPERT level");
          this.difficultyHard = false;
          this.difficultyExpert = true;
        }
      }, 1000);

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

        let positionGreenCar = Math.round(
          this.$refs.greenCar.getBoundingClientRect().top
        );

        if (
          this.leftStyle <= 40 &&
          positionRedCar > 400 &&
          positionRedCar < 470
        ) {
          clearInterval(timerInterval);
          this.stopGame();
          return;
        }

        if (
          this.leftStyle >= 52 &&
          positionBlueCar > 400 &&
          positionBlueCar < 470
        ) {
          clearInterval(timerInterval);
          this.stopGame();
          return;
        }

        if (
          this.leftStyle >= 41 &&
          this.leftStyle <= 51 &&
          positionGreenCar > 400 &&
          positionGreenCar < 470
        ) {
          clearInterval(timerInterval);
          this.stopGame();
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
  /* top: 50%; */
}

.parentDiv {
  position: relative;
  width: 100%;
  height: 100%;
}

input {
  /* width: 100vw; */
  width: 100%;
  height: 140vw;
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
.normal.gameOn .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 4s;
  -webkit-animation-name: car-vertical-normal;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.normal.gameOn .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 4s;
  -webkit-animation-name: car-vertical-normal;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}

.normal.gameOn .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 4s;
  -webkit-animation-name: car-vertical-normal;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 3s;
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

.medium.gameOn .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 3s;
  -webkit-animation-name: car-vertical-medium;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.medium.gameOn .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 3s;
  -webkit-animation-name: car-vertical-medium;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}
.medium.gameOn .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 3s;
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
.hard.gameOn .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 2s;
  -webkit-animation-name: car-vertical-hard;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.hard.gameOn .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 2s;
  -webkit-animation-name: car-vertical-hard;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}

.hard.gameOn .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 2s;
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
.expert.gameOn .blueCar {
  background: url("../assets/blueCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 58%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car-vertical-expert;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 2s;
}
.expert.gameOn .redCar {
  background: url("../assets/redCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 30%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car-vertical-expert;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 1s;
}

.expert.gameOn .greenCar {
  background: url("../assets/greenCar.png");
  background-size: cover;
  background-repeat: no-repeat;
  width: 110px;
  height: 200px;
  position: absolute;
  left: 44%;
  -webkit-animation: 1.5s;
  -webkit-animation-name: car-vertical-expert;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: normal;
  -webkit-animation-delay: 3s;
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