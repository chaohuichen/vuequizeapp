<template>
  <div>
    <app-header></app-header>
    <slot></slot>
    <app-footer></app-footer>
  </div>
</template>

<script>
import Header from "./Header";
import Footer from "./Footer";
export default {
  components: {
    "app-header": Header,
    "app-footer": Footer,
  },
  data() {
    return {
      title: "Quiz Application",
    };
  },
};
</script>

<style>
/* div {
  display: flex;
  flex-direction: row;
} */
</style>
