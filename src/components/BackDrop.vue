<template>
  <div>
    <canvas ref="starsCanvas"></canvas>
  </div>
</template>

<script>
export default {
  mounted() {
    this.setupStars();
    this.updateStars();
    window.addEventListener("resize", this.setupStars);
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.setupStars);
  },
  methods: {
    setupStars() {
      const canvas = this.$refs.starsCanvas;
      this.screen = {
        w: window.innerWidth,
        h: window.innerHeight,
        c: [window.innerWidth * 0.5, window.innerHeight * 0.5],
      };
      canvas.width = this.screen.w;
      canvas.height = this.screen.h;
      this.stars = [];
      for (let i = 0; i < this.params.number; i++) {
        this.stars[i] = new Star(canvas, this.screen, this.params);
      }
    },
    updateStars() {
      const canvas = this.$refs.starsCanvas;
      const ctx = canvas.getContext("2d");
      ctx.fillStyle = "black";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      this.stars.forEach((s) => {
        s.show(ctx);
        s.move();
      });
      window.requestAnimationFrame(this.updateStars);
    },
  },
  data() {
    return {
      screen: {},
      stars: [],
      params: { speed: 3, number: 1000, extinction: 4 },
    };
  },
};

function Star(canvas, screen, params) {
  this.x = Math.random() * canvas.width;
  this.y = Math.random() * canvas.height;
  this.z = Math.random() * canvas.width;

  this.move = function () {
    this.z -= params.speed;
    if (this.z <= 0) {
      this.z = canvas.width;
    }
  };

  this.show = function (ctx) {
    let x, y, rad, opacity;
    x = (this.x - screen.c[0]) * (canvas.width / this.z);
    x = x + screen.c[0];
    y = (this.y - screen.c[1]) * (canvas.width / this.z);
    y = y + screen.c[1];
    rad = canvas.width / this.z;
    opacity = rad > params.extinction ? 1.5 * (2 - rad / params.extinction) : 1;

    ctx.beginPath();
    ctx.fillStyle = "rgba(255, 255, 255, " + opacity + ")";
    ctx.arc(x, y, rad, 0, Math.PI * 2);
    ctx.fill();
  };
}
</script>

<style scoped>
canvas {
  position: fixed;
  z-index: -2;
}
</style>
