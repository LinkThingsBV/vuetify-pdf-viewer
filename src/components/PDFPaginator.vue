<template>
  <div class="pdf-paginator">
    <template v-if="pageCount">
      <input
        :value="value"
        min="1"
        :max="pageCount"
        type="number"
        @input="input"
      > / <span>{{ pageCount }}</span>
    </template>
    <input v-else type="number">
  </div>
</template>

<script>
export default {
  name: 'PDFPaginator',

  props: {
    value: {
      type: Number,
      default: null
    },
    pageCount: {
      type: Number,
      default: null
    }
  },

  watch: {
    pageCount() {
      this.$emit('input', 1);
    }
  },

  methods: {
    input(event) {
      this.$emit('input', parseInt(event.target.value, 10));
    }
  }
}
</script>

<style>
.pdf-paginator {
  color: #FB8C00;
  font-weight: bold;
}
.pdf-paginator input {
  width: 2em;
  padding: 0.3em;
}
</style>
