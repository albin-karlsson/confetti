<template>
  <ColorPicker @color-picked="setColor"/> <Canvas :color="color"
/></template>

<script>
import ColorPicker from "./components/ColorPicker.vue";
import Canvas from "./components/Canvas.vue";

export default {
  name: "App",
  components: {
    ColorPicker,
    Canvas,
  },
  data() {
    return {
      color: { r: 0, g: 0, b: 0 },
    };
  },
  methods: {
    setColor(color) {
      this.color = color;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
