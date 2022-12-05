<template>
  <div class="pdf-close">
    <v-btn
      icon
      dark
      color="primary"
      @click="closeViewer"
    >
      <v-icon>{{ mdiCloseBoxOutline }}</v-icon>
    </v-btn>
  </div>
</template>

<script>
import { mdiCloseBoxOutline } from "@mdi/js";

export default {
    name: "PDFClose",

    data: () => ({
        mdiCloseBoxOutline
    }),

    props: {},

    computed: {},

    methods: {
        closeViewer(scale) {
            this.$emit("close-viewer");
        }
    }
};
</script>

<style>
.pdf-close {
}
</style>