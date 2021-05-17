<template>
  <canvas
    @mousemove="draw"
    ref="canvas"
    class="w-full h-screen bg-gray-100 fixed"
  ></canvas>
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
      if (this.color.r == 0 && this.color.g == 0 && this.color.b == 0) {
        return;
      }

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
      this.y = (e.offsetY / this.bottom) * 165;
    },
    createCanvas() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");

      return ctx;
    },
    createCircle(ctx) {
      const randomDegrees = Math.floor(Math.random() * 360);
      const randomSize = Math.floor(Math.random() * (7 - 3)) + 3;
      ctx.translate(this.x, this.y);
      ctx.rotate(randomDegrees);
      ctx.fillRect(0, 0, randomSize, randomSize);
      ctx.rotate(-randomDegrees);
      ctx.translate(-this.x, -this.y);
      ctx.fillStyle = this.getHex();
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
