<template>
  <div class="pdf-viewer">
    <nav class="pdf-viewer__header box-shadow">
      <v-col
        align="left"
        cols="2"
        class="pa-0 px-2"
      >
        <div class="pdf-preview-toggle">
          <v-btn
            icon
            color="primary"
            @click.prevent.stop="togglePreview"
          >
            <v-icon>{{ mdiFilterVariant }}</v-icon>
          </v-btn>
        </div>
      </v-col>
      <v-col
        align="center"
        class="pa-0"
      >
        <PDFZoom
          :scale="scale"
          class="header-item"
          @change="updateScale"
          @fit="updateFit"
        />
      </v-col>
      <v-col
        align="right"
        cols="2"
        class="hidden-sm-and-down pa-0 px-5"
      >
        <PDFPaginator
          v-model="currentPage"
          :page-count="pageCount"
          class="header-item"
        />
      </v-col>
      <v-col
        align="right"
        cols="2"
        class="hidden-sm-and-down pa-0 px-5"
      >
        <PDFClose
          v-on:close-viewer="closePdfViewer"
          class="header-item"
        />
      </v-col>
      <slot name="header" />
    </nav>

    <PDFData
      class="pdf-viewer__main"
      :url="url"
      @page-count="updatePageCount"
      @page-focus="updateCurrentPage"
      @document-rendered="onDocumentRendered"
      @document-errored="onDocumentErrored"
    >
      <template v-slot:preview="{pages}">
        <PDFPreview
          v-show="isPreviewEnabled"
          class="pdf-viewer__preview"
          v-bind="{pages, scale, currentPage, pageCount, isPreviewEnabled}"
        />
      </template>

      <template v-slot:document="{pages}">
        <PDFDocument
          class="pdf-viewer__document"
          :class="{ 'preview-enabled': isPreviewEnabled }"
          v-bind="{pages, scale, optimalScale, fit, currentPage, pageCount, isPreviewEnabled}"
          @scale-change="updateScale"
        />
      </template>
    </PDFData>
  </div>
</template>

<script>
import PDFDocument from "./PDFDocument";
import PDFData from "./PDFData";
import PDFPaginator from "./PDFPaginator";
import PDFPreview from "./PDFPreview";
import PDFZoom from "./PDFZoom";
import PDFClose from "./PDFClose";
import { mdiFilterVariant } from "@mdi/js";

function floor(value, precision) {
    const multiplier = Math.pow(10, precision || 0);
    return Math.floor(value * multiplier) / multiplier;
}

export default {
    name: "PDFViewer",

    components: {
        PDFDocument,
        PDFData,
        PDFPaginator,
        PDFPreview,
        PDFZoom,
        PDFClose
    },

    props: {
        url: {
            default: undefined
        }
    },

    data: () => ({
        mdiFilterVariant,
        scale: undefined,
        optimalScale: undefined,
        fit: undefined,
        currentPage: 1,
        pageCount: undefined,
        isPreviewEnabled: true
    }),

    watch: {
        url() {
            this.currentPage = undefined;
        }
    },

    mounted() {
        document.body.classList.add("overflow-hidden");
    },

    methods: {
        onDocumentRendered() {
            this.$emit("document-errored", this.url);
        },

        onDocumentErrored(e) {
            this.$emit("document-errored", e);
        },

        updateScale({ scale, isOptimal = false }) {
            const roundedScale = floor(scale, 2);
            if (isOptimal) this.optimalScale = roundedScale;
            this.scale = roundedScale;
        },

        updateFit(fit) {
            this.fit = fit;
        },

        updatePageCount(pageCount) {
            this.pageCount = pageCount;
        },

        updateCurrentPage(pageNumber) {
            this.currentPage = pageNumber;
        },

        togglePreview() {
            this.isPreviewEnabled = !this.isPreviewEnabled;
        },

        closePdfViewer() {
            this.$emit("close-pdf-viewer");
        }
    }
};
</script>

<style scoped>
nav {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    padding: 1em;
    position: relative;
}
.header-item {
    margin: 0;
}

.pdf-viewer .pdf-viewer__document,
.pdf-viewer .pdf-viewer__preview {
    top: 70px;
}

.pdf-viewer__preview {
    display: block;
    width: 15%;
    right: 85%;
}

.pdf-viewer__document {
    top: 70px;
    width: 100%;
    left: 0;
}

.pdf-viewer__document.preview-enabled {
    width: 85%;
    left: 15%;
}

@media print {
    header {
        display: none;
    }
}
</style>
