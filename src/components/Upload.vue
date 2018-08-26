<template>
  <div class="container">
    <form enctype="multipart/form-data" novalidate>
      <h1>Upload SPICE netlist</h1>
      <div class="dropbox">
        <input type="file" :name="uploadFieldName" @change="filesChange($event.target.name,
        $event.target.files)" accept="*/*" class="input-file">
        <p v-html="uploadText">
        </p>
      </div>
    </form>

    <Modal v-show="modalVisibility" @close="hideModal" @send-params="sendData"></Modal>
  </div>
</template>

<script>
import Modal from './Modal'

const STATUS_INITIAL = 0, STATUS_SAVING = 1, STATUS_SUCCESS = 2, STATUS_FAILED = 3;

export default {
  name: 'Upload',
  components: { Modal },

  data() {
    return {
      uploadedFiles: [],
      initialText: "Drag your file here to begin<br> or click to browse",
      errorText: "An error has occured. Please refer to the help page for further information. <br>",
      uploadText: "Drag your file here to begin<br> or click to browse",
      uploadFieldName: 'netlist',
      modalVisibility: false,
      params: new FormData()
    }
  },

  methods: {
    isInitial() {
      this.uploadText = this.initialText;
    },

    isFailed(error) {
      this.uploadText = this.errorText + error;
    },

    showModal() {
      this.modalVisibility = true;
    },

    hideModal() {
      this.modalVisibility = false;
    },

    reset() {
      this.uploadedFiles = [];
      this.uploadError = null;
    },

    save(formData) {
      let vm = this;
      const BASE_URL = 'http://127.0.0.1:3000/upload';
      this.currentStatus = STATUS_SAVING;

      this.$http.post(BASE_URL, formData)
      .then(function(response) {
        vm.prepareDownload(response);
        vm.isInitial();
      })
      .catch(function(error) {
        vm.isFailed(error);
      });
    },

    prepareDownload(data) {
      let blob = new Blob([data.data.file], { type: "octet/stream"});
      this.$emit("fileForDownload", blob);
    },

    filesChange(fieldName, fileList) {
      if (!fileList.length) return;

      Array
      .from(Array(fileList.length).keys())
      .map(x => {
        this.params.append(fieldName, fileList[x], fileList[x].name);
      });

      this.showModal();
    },

    sendData(values) {
      for (var key in values) {
        this.params.append(key, values[key]);
      }
      this.save(this.params);
    }
  },

  mounted() {
    this.reset();
  },
}
</script>

<style scoped>
  .dropbox {
    outline: 2px dashed grey; /* the dash box */
    outline-offset: -10px;
    background: lightcyan;
    color: dimgray;
    padding: 10px 10px;
    min-height: 500px; /* minimum height */
    position: relative;
    cursor: pointer;
    display:flex;
    justify-content:center;
    align-items:center;
    text-align: center;
  }

  .input-file {
    opacity: 0; /* invisible but it's there! */
    width: 100%;
    height: 500px;
    position: absolute;
    cursor: pointer;
  }

  .dropbox:hover {
    background: lightblue; /* when mouse over to the drop zone, change color */
  }

  .dropbox p {
    font-size: 1.2em;
  }
</style>
