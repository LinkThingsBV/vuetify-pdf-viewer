# vuetify-pdf-viewer

Simple PDF viewer. Needs vuetify and @mdi/js.

This project demonstrates how a simple PDF viewer with simple pdf controls. It is implemented using Vue, Vuetify and PDF.js. 

## Version

1.0.0 : Blurred pages corrected

## Setup

To install the plugin inside your project :

```
$ npm install vuetify-pdf-viewer

$ npm install vuetify
$ npm install @mdi/js -D

```

And, in your main.js file :

```
import Vuetify from "vuetify";

```

Then, copy this code where you want inside you project :

``` 
<template>
  <v-carousel :show-arrows="false" :hide-delimiters="true" class="carousel">
    <VuetifyPdf
      :url="url"
      v-on:close-pdf-viewer="closeModal" //optional -- use if some actions needs to be perform when the pdf viewer is closed
    />
  </v-carousel>
</template>

<script>
import VuetifyPdf from "vuetify-pdfjs/src/App.vue";

export default {
  components: {
    VuetifyPdf
  },
    data: () => ({
    url:'<link to your pdf file>'
  })
}
</script>

<style>
.carousel {
  background-color: #606f7b;
}
</style>

```
