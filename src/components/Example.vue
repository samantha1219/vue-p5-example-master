<template>
  <div>
    <vue-p5
      @preload="preload"
      @setup="setup"
      @draw="draw"
      @keypressed="keyPressed"
      @mousemoved="mouseMoved"
      @mousedragged="mouseDragged"
    >
    </vue-p5>
  </div>
</template>

<script>
import VueP5 from "vue-p5";

export default {
  name: "p5-example",
  components: {
    "vue-p5": VueP5,
  },
  data: () => ({}),
  computed: {},
  methods: {
    setup(sk) {
      sk.createCanvas(710, 400, sk.WEBGL);
    },
    draw(sk) {
      sk.background(250);
      sk.rotateY(sk.frameCount * 0.01);
      for (var j = 0; j < 5; j++) {
        sk.push();
        for (var i = 0; i < 80; i++) {
          sk.translate(
            sk.sin(sk.frameCount * 0.001 + j) * 100,
            sk.sin(sk.frameCount * 0.001 + j) * 100,
            i * 0.1
          );
          sk.rotateZ(sk.frameCount * 0.002);
          sk.push();
          sk.sphere(8, 6, 4);
          sk.pop();
        }
        sk.pop();
      }
    },

    keyPressed({ keyCode }) {
      // 'g' key
      if (keyCode === 71) {
        this.toggleGreen();
      }
    },
    mouseMoved({ mouseX, mouseY, pmouseX, pmouseY }) {
      this.pushLine({ mouseX, mouseY, pmouseX, pmouseY, color: 0 });
    },
    mouseDragged({ mouseX, mouseY, pmouseX, pmouseY }) {
      this.pushLine({ mouseX, mouseY, pmouseX, pmouseY, color: 255 });
    },
    toggleRed() {
      this.red = 255 - this.red;
    },
    toggleGreen() {
      this.green = 255 - this.green;
    },
    pushLine(line) {
      let lines = this.lines;
      lines.push(line);
      this.lines = lines.slice(-100);
    },
  },
};
</script>
