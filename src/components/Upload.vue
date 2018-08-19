<template>
  <div class="container">
    <form enctype="multipart/form-data" novalidate v-if="isInitial || isSaving">
      <h1>Upload SPICE netlist</h1>
      <div class="dropbox">
        <input type="file" :name="uploadFieldName" :disabled="isSaving" @change="filesChange($event.target.name,
        $event.target.files)" accept="*/*" class="input-file">
        <p v-if="isInitial">
          Drag your file here to begin<br> or click to browse
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
        uploadError: null,
        currentStatus: null,
        uploadFieldName: 'netlist',
        modalVisibility: false,
        params: new FormData()
      }
    },
    computed: {
      isInitial() {
        return this.currentStatus === STATUS_INITIAL;
      }, isSaving() {
        return this.currentStatus === STATUS_SAVING;
      },
      isSuccess() {
        return this.currentStatus === STATUS_SUCCESS;
      },
      isFailed() {
        return this.currentStatus === STATUS_FAILED;
      }
    },
    methods: {

    showModal() {
      this.modalVisibility = true;
    },

    hideModal() {
      this.modalVisibility = false;
    },
      reset() {
        // reset form to initial state
        this.currentStatus = STATUS_INITIAL;
        this.uploadedFiles = [];
        this.uploadError = null;
      },
      save(formData) {
        const BASE_URL = 'http://127.0.0.1:3000/upload';
        this.currentStatus = STATUS_SAVING;

        this.$http.post(BASE_URL, formData)
          .then(function(response) {
            this.uploadedFiles = [].concat(x);
            this.currentStatus = STATUS_SUCCESS;
          })
          .then(function(error) {
            this.uploadError = err.response;
            this.currentStatus = STATUS_FAILED;
          });

        // upload(formData);
          // .then(x => {
          //   this.uploadedFiles = [].concat(x);
          //   this.currentStatus = STATUS_SUCCESS;
          // })
          // .catch(err => {
          //   this.uploadError = err.response;
          //   this.currentStatus = STATUS_FAILED;
          // });
      },
      filesChange(fieldName, fileList) {
        // handle file changes
        // const formData = new FormData();

        if (!fileList.length) return;

        // append the files to FormData
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