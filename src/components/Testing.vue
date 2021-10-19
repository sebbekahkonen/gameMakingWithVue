<template>
  <div>
    <h1>{{ newfunc() }}</h1>
    <input type="text" placeholder="Name" v-model="name" />
    <p>name is: {{ name }}</p>
    <button></button>
  </div>
</template>

  <script>
import { ref, onMounted } from "vue";
export default {
  data() {
    return {
      greeting: "Greetings ",
      name: "",
      el,
    };
  },
  methods: {
    newfunc() {
      return this.greeting + this.name;
    },
  },
};
</script>

<style scoped>
</style>
