<template>
  <div @mousemove="draw" class="w-full h-screen bg-gray-100 fixed">
    <canvas ref="canvas" class=""></canvas>
  </div>
</template>

<script>
export default {
  name: "Canvas",
  props: ["color"],
  data() {
    return {
      x: 0,
      y: 0,
      right: 0,
      bottom: 0,
    };
  },
  methods: {
    draw(e) {
      this.setBoundingRect();
      this.calculateLocation(e);
      this.createCircle(this.createCanvas());
    },
    setBoundingRect() {
      let boundingRect = this.$refs.canvas.getBoundingClientRect();
      this.right = boundingRect.right;
      this.bottom = boundingRect.bottom;
    },
    calculateLocation(e) {
      this.x = (e.offsetX / this.right) * 300;
      this.y = (e.offsetY / this.bottom) * 180;
    },
    createCanvas() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");

      return ctx;
    },
    createCircle(ctx) {
      ctx.fillRect(this.x, this.y, 5, 5);

      ctx.fillStyle = this.getHex();

      console.log(this.getHex());
      ctx.stroke();
    },
    getHex() {
      var red = this.rgbToHex(this.color.r);
      var green = this.rgbToHex(this.color.g);
      var blue = this.rgbToHex(this.color.b);
      return "#" + red + green + blue;
    },
    rgbToHex(number) {
      var hex = Number(number).toString(16);
      if (hex.length < 2) {
        hex = "0" + hex;
      }
      return hex;
    },
  },
};
</script>

<style scoped>
canvas {
  width: 100%;
  height: 100%;
}
</style>
