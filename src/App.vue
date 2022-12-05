<template>
  <div id="app">
    <PDFViewer
      v-bind="{url}"
      @document-errored="onDocumentErrored"
      v-on:close-pdf-viewer="closePdfViewer"
    >
      <PDFUploader
        v-if="enableUploader"
        slot="header"
        :document-error="documentError"
        class="header-item"
        @updated="urlUpdated"
      />
    </PDFViewer>
  </div>
</template>

<script>
import PDFUploader from "./components/PDFUploader.vue";
import PDFViewer from "./components/PDFViewer.vue";

export default {
    name: "App",

    components: {
        PDFUploader,
        PDFViewer
    },

    props: {
        url: {
            default:
                "https://cors-anywhere.herokuapp.com/http://mms2.ensmp.fr/mat_paris/structure/transparents/Amphi_Intro_Structures_2008.pdf"
        }
    },

    data() {
        return {
            documentError: undefined,
            enableUploader: process.env.VUE_APP_UPLOAD_ENABLED === "true"
        };
    },

    methods: {
        urlUpdated(url) {
            this.documentError = undefined;
            this.url = url;
        },
        onDocumentErrored(e) {
            this.documentError = e.text;
        },
        closePdfViewer() {
            this.$emit("close-pdf-viewer");
        }
    }
};
</script>

<style>
label.form {
    color: white;
    font-family: Monaco, "Courier New", Courier, monospace;
    font-weight: bold;
    margin-bottom: 2em;
    display: block;
}
input {
    padding: 0.45em;
    font-size: 1em;
}
.error {
    border: 1px solid red;
    background: pink;
    color: red;
    padding: 0.5em 3em;
    display: inline;
}

.box-shadow {
    box-shadow: 0 15px 30px 0 rgba(0, 0, 0, 0.11),
        0 5px 15px 0 rgba(0, 0, 0, 0.08);
    z-index: 2;
}
.overflow-hidden {
    overflow: hidden;
}

@media print {
    body {
        background-color: transparent;
    }
    #app {
        margin: 0;
        padding: 0;
    }
}
</style>
