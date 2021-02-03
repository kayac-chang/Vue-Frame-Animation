<template>
  <div>
    <img
      v-for="(frame, index) in frames"
      v-show="currentFrame === index"
      :key="index"
      :src="frame"
      :alt="`${frame}${index}`"
      @load="() => (loaded += 1)"
    />
  </div>
</template>

<script>
async function nextFrame() {
  const start = performance.now();

  return new Promise((resolve) =>
    requestAnimationFrame((end) => resolve(end - start))
  );
}

export default {
  name: "FrameAnimation",
  props: {
    frames: {
      type: Array,
      default: () => [],
    },
    loop: {
      type: Boolean,
      default: false,
    },
    autoplay: {
      type: Boolean,
      default: false,
    },
    fps: {
      type: Number,
      default: 30,
    },
  },
  data() {
    return {
      currentFrame: 0,
      playing: false,
      loaded: 0,
    };
  },
  mounted() {
    this.autoplay && this.play();
  },
  computed: {
    isLoaded() {
      return this.loaded === this.frames.length;
    },
    isFinalFrame() {
      return this.currentFrame >= this.frames.length - 1;
    },
    interval() {
      return 1000 / this.fps;
    },
  },
  methods: {
    async play() {
      this.playing = true;

      while (!this.isLoaded) {
        await nextFrame();
      }

      while (this.playing) {
        await this.run();
      }
    },
    async run(delta = 0) {
      delta += await nextFrame();

      if (delta < this.interval) {
        return this.run(delta);
      }

      if (this.loop && this.isFinalFrame) {
        this.currentFrame = 0;
      } else {
        this.currentFrame += 1;
      }
    },
    stop() {
      this.playing = false;
    },
  },
};
</script>

<style scoped>
img {
  width: 100%;
  max-height: auto;
}
</style>
