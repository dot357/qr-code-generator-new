<template>
  <Transition name="fade">
    <div class="container" v-if="computedQrCodes.length > 0">
      <button class="btn-classic yellow-grad" @click="downloadSelected">
        Download
      </button>

      <SingleQr
        v-for="(item, index) in computedQrCodes.sort((a,b) => a.id-b.id)"
        :key="index"
        :item="item"
        :id="index"
        @prop-changed="item = $event"
      />
    </div>
  </Transition>
</template>

<script>
import SingleQr from "./SingleQr.vue";
import JSzip from "jszip";
import { saveAs } from "file-saver";


export default {
  props: {
    qrCodes: {
      type: Object,
    },
  },
  components: {
    SingleQr,
  },
  methods: {
    downloadSelected() {
      const selected = this.computedQrCodes.filter((e) => e.selected === true);
      
      var zip = new JSzip();
      var img = zip.folder("qrCodes");
      console.log(selected.length)
      selected.forEach((e, index) => {
        
      
        // download.click();
        let imgData = e.qr[0].replace("data:image/png;base64,", "");
        
        img.file(`${e.data}.png`, imgData, { base64: true });
      });
      zip.generateAsync({ type: "blob" }).then(function (content) {
        // see FileSaver.js
        saveAs(content, "qrCodes.zip");
      });

    },
  },
  computed: {
    computedQrCodes: {
      get() {
        return this.qrCodes;
      },
      set(value) {
        this.$emit("prop-has-changed", value);
      },
    },
  },
  watch: {
    computedQrCodes(newValue) {
      console.log(newValue);
    },
  },
};
</script>

<style scoped>
.container {
  margin-top: 7.5rem;
  overflow-wrap: break-word;
  height: auto !important;
  position: relative;
}

button {
  position: fixed;
  bottom: 3rem;
  right: 1rem;
  z-index: 999;
  padding: 12px 48px;
  font-weight: bold;
  color: #705a08;
  text-transform: uppercase;
  border: 1px solid #fee000;
  box-shadow: 0px -4px 25px #fad87c, 0px 4px 25px #f8eb79;
}
</style>
