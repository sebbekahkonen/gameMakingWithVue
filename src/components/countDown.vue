<template>
  <div class="countDownDiv">
    <p>{{ countDownNumber }}</p>
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
.countDownDiv {
  width: 100px;
  height: 100px;
  background-color: green;
}
</style>