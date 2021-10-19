<template>
  <div>
    <input
      ref="el"
      type="text"
      @keydown.left="moveLeft()"
      @keydown.right="moveRight()"
      v-model="leftStyle"
    />
    <div :style="{ left: `${leftStyle}%` }" class="character"></div>
    <div class="redCar"></div>
    <div class="blueCar"></div>
  </div>
</template>


<script>
import { ref, onMounted } from "vue";
export default {
  setup() {
    const leftStyle = ref(35);
    const el = ref();
    onMounted(() => {
      setTimeout(() => {
        el.value.focus();
      }, 10);
    });

    return {
      el,
      name,
      leftStyle,
    };
  },
  methods: {
    moveLeft() {
      console.log("moveLEft", this.leftStyle);
      if (this.leftStyle == 60) {
        console.log("thismovedLeft");
        this.leftStyle = this.leftStyle - 25;
      } else {
        console.log("hey");
        return;
      }
    },
    moveRight() {
      console.log("moveRight", this.leftStyle);
      if (this.leftStyle == 35) {
        console.log("thisMovedRight");
        this.leftStyle = this.leftStyle + 25;
      } else {
        console.log("hey");
        return;
      }
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

.redCar {
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

.blueCar {
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