<template>
  <div id="app">
    <h1>Frame Animation with VueJS!</h1>

    <div class="flex justify-center">
      <FrameAnimation class="w-48" :frames="frames" :fps="32" autoplay loop />
    </div>
  </div>
</template>

<script>
import FrameAnimation from "./components/FrameAnimation";

function range(n) {
  return [...Array(n).keys()];
}

export default {
  name: "App",
  components: {
    FrameAnimation,
  },
  computed: {
    frames() {
      return range(21).map((idx) => require(`./assets/Frame_${idx}.png`));
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.flex {
  display: flex;
}
.justify-center {
  justify-content: center;
}

.w-48 {
  width: 12rem;
}
</style>
