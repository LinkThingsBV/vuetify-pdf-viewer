<template>
  <div class="pdf-zoom">
    <v-btn icon color="primary" @click.prevent.stop="zoomIn">
      <v-icon>{{ mdiMagnifyPlusOutline }}</v-icon>
    </v-btn>
    <v-btn icon color="primary" @click.prevent.stop="zoomOut">
      <v-icon>{{ mdiMagnifyMinusOutline }}</v-icon>
    </v-btn>
    <v-btn icon color="primary" @click.prevent.stop="fitWidth">
      <v-icon>{{ mdiArrowExpand }}</v-icon>
    </v-btn>
    <v-btn icon color="primary" @click.prevent.stop="fitAuto">
      <v-icon>{{ mdiFitToPageOutline }}</v-icon>
    </v-btn>
  </div>
</template>

<script>
import { mdiMagnifyPlusOutline, mdiMagnifyMinusOutline, mdiArrowExpand, mdiFitToPageOutline  } from '@mdi/js';

export default {
  name: 'PDFZoom',

  data: () => ({
    mdiMagnifyPlusOutline,
    mdiMagnifyMinusOutline,
    mdiArrowExpand,
    mdiFitToPageOutline
  }),

  props: {
    scale: {
      type: Number,
      default: 1.0
    },
    increment: {
      type: Number,
      default: 0.25
    }
  },

  computed: {
    isDisabled() {
      return !this.scale;
    }
  },

  methods: {
    zoomIn() {
      this.updateScale(this.scale + this.increment);
    },

    zoomOut() {
      if (this.scale <= this.increment) return;
      this.updateScale(this.scale - this.increment);
    },

    updateScale(scale) {
      this.$emit('change', {scale});
    },

    fitWidth() {
      this.$emit('fit', 'width');
    },

    fitAuto() {
      this.$emit('fit', 'auto');
    }
  }
}
</script>

<style>
.pdf-zoom a {
  float: left;
  cursor: pointer;
  display: block;
  border: 1px #333 solid;
  background: white;
  color: #333;
  font-weight: bold;
  line-height: 1.5em;
  width: 1.5em;
  height: 1.5em;
  font-size: 1.5em;
}
</style>
