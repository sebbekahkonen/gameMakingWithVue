<script setup>
import { ref } from "vue";

defineProps({
  msg: String,
});

const count = ref(0);

function testfunction() {
  console.log("hello this works");
}
</script>

<template>
  <button
    @click="
      {
        testfunction;
      }
    "
  >
    Click here!
  </button>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
