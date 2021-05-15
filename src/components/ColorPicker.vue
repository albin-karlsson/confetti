<template>
  <div
    ref="line"
    class="w-full h-3 relative cursor-pointer"
    style="background-image:linear-gradient(90deg,red 0,#ff0 17%,#0f0 33%,#0ff 50%,#00f 67%,#f0f 83%,red)"
    @mousemove="mousemoveLine"
  >
    <div
      ref="lineCursor"
      class="h-4 w-4 border border-gray-200 rounded-full bg-white absolute -left-2 pointer-events-none"
      style="top:-2px;box-shadow:2px 2px 2px 0 rgb(0 0 0 / 20%)"
    ></div>
  </div>
</template>

<script>
export default {
  name: "ColorPicker",
  data() {
    return {
      dragStartColor: null,
      draggingLineCursor: false,
      lineLeft: 0,
      lineRight: 0,
      lineColorData: {
        r: 0,
        g: 0,
        b: 0,
      },
      linewidth: 100,
    };
  },
  computed: {
    color() {
      return `rgba(${this.lineColorData.r}, ${this.lineColorData.g}, ${this.lineColorData.b})`;
    },
  },
  methods: {
    mousemoveLine(e) {
      this.dragStartColor = this.color;
      this.draggingLineCursor = true;
      this.setSizePoses();
      this.lineCursor = this.$refs.lineCursor;
      this.lineCursor.style.transform = `translate(${e.offsetX}px, 0px)`;
      this.calculateLineColor(e.offsetX);
      this.$emit(`color-picked`, this.lineColorData);
      e.stopPropagation();
      e.preventDefault();
    },
    setSizePoses() {
      let boundingRect = this.$refs.line.getBoundingClientRect();
      this.lineLeft = boundingRect.left;
      this.lineRight = boundingRect.right;
    },
    calculateLineColor(linePos) {
      let perc = linePos / this.lineRight;
      let value = perc % (1 / 6);
      let colorPerc = value / (1 / 6);
      let percRed = 1;
      let percGreen = 1;
      let percBlue = 1;
      if (perc < 1 / 6) {
        percGreen = colorPerc;
        percBlue = 0;
      } else if (perc < 2 / 6) {
        percRed = 1 - colorPerc;
        percBlue = 0;
      } else if (perc < 3 / 6) {
        percRed = 0;
        percBlue = colorPerc;
      } else if (perc < 4 / 6) {
        percRed = 0;
        percGreen = 1 - colorPerc;
      } else if (perc < 5 / 6) {
        percRed = colorPerc;
        percGreen = 0;
      } else {
        percGreen = 0;
        percBlue = 1 - colorPerc;
      }
      this.lineColorData.r = Math.min(
        255,
        Math.max(0, Math.round(percRed * 255))
      );
      this.lineColorData.g = Math.min(
        255,
        Math.max(0, Math.round(percGreen * 255))
      );
      this.lineColorData.b = Math.min(
        255,
        Math.max(0, Math.round(percBlue * 255))
      );
    },
  },
};
</script>

<style scoped>
div {
  height: 80px;
}
</style>
