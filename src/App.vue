<template>
  <div id="app" ref="app" class="bg-gray-600 relative p-4">
    <section ref="space" class="__space bg-gray-200/20">
      <div
        class="
          __box
          grid
          place-items-center
          rounded-full
          backdrop-blur-md
          bg-white/30
        "
        :style="inlineStyle(i)"
        v-for="(box, i) in boxes"
        :key="i"
      >
        {{ box }}
      </div>
    </section>
    <section class="__scroll bg-cyan-400/20" v-if="false"></section>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import { random } from 'lodash';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  data() {
    return {
      spaceWidth: 0,
      spaceHeight: 0,
      boxWidth: 440,
      boxHeight: 440,
      length: { length: 10 },
      isRandom: true,
    };
  },
  computed: {
    boxes() {
      return Array.from(this.length, (x, i) => i + 1);
    },
    boundsX() {
      return this.spaceWidth - this.boxWidth;
    },
    boundsY() {
      return this.spaceHeight - this.boxHeight;
    },
  },

  mounted() {
    this.setSize();
    this.centerSpace();

    const vm = this;
    window.onresize = () => {
      vm.setSize();
    };
  },
  methods: {
    setSize() {
      this.spaceWidth = this?.$refs?.space?.offsetWidth;
      this.spaceHeight = this?.$refs?.space?.offsetHeight;
    },
    centerSpace() {
      const _app = this.$refs.app;
      if (!_app) return;
      _app.scrollTop = this.spaceHeight / 4;
      _app.scrollLeft = this.spaceWidth / 4;
    },
    transfromX(i) {
      return this.isRandom ? random(0, this.boundsX) : this.boundsX / 2;
    },
    transfromY(i) {
      return this.isRandom ? random(0, this.boundsY) : this.boundsY / 2;
    },
    transfromZ(i) {
      return this.isRandom ? -i * random(0, 10) : -i * 10;
    },
    inlineStyle(i) {
      const x = this.transfromX(i);
      const y = this.transfromY(i);
      const z = this.transfromZ(i);
      const scale = this.isRandom ? random(0.5, 1.8) : 1;
      const transform = `translate3d(${x}px, ${y}px, ${z}px) scale(${scale})`;
      return { transform };
    },
  },
};
</script>

<style>
#app {
  height: 100vh;
  overflow-y: auto;
  overflow-x: auto;
  perspective: 10px;
  /* perspective-origin: center; */
}

.__space {
  position: relative;
  transform-style: preserve-3d;
  z-index: -1;
  width: 200%;
  height: 200%;
}

.__box {
  width: 440px;
  height: 440px;
  position: absolute;
  z-index: -1;
}

.__scroll {
  height: 10000px;
}
</style>
