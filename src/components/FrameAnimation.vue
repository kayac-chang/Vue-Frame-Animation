<template>
  <div class="wrapper">
    <img
      v-for="(frame, index) in frames"
      v-show="currentFrame === index"
      :key="index"
      :src="frame"
      :alt="`${frame}${index}`"
    />
  </div>
</template>

<script>
async function nextFrame() {
  return new Promise((resolve) => requestAnimationFrame(resolve));
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
  },
  data() {
    return {
      currentFrame: 0,

      playing: false,
    };
  },
  mounted() {
    this.autoplay && this.play();
  },
  computed: {
    isFinalFrame() {
      return this.currentFrame < this.frames.length - 1;
    },
  },
  methods: {
    async play() {
      this.playing = true;

      while (this.playing && this.isFinalFrame) {
        this.currentFrame += 1;

        await nextFrame();
      }

      if (this.loop) {
        this.currentFrame = 0;

        this.play();
      }
    },
    stop() {
      this.playing = false;
    },
  },
};
</script>

<style scoped>
.wrapper {
  width: 12rem;
}

img {
  width: 100%;
  max-height: auto;
}
</style>
