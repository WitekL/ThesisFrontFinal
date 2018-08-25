<template>
  <div id="app">
    <button v-on:click="showHelp">Help</button>
    <div id='circuit-panel'>
      <div class="scheme-type" @change="clearValues">
        <label>Pi</label><input type="radio" v-model="viewType" value="Pi">
        <label>T</label><input type="radio" v-model="viewType" value="T">
        <label>Upload</label><input type="radio" v-model="viewType" value="Upload">
      </div>
      <PiType v-if="viewType === 'Pi'" @element-quantities="handleElementQuantities($event)"></PiType>
      <TType v-if="viewType === 'T'" @element-quantities="handleElementQuantities($event)"></TType>
      <Upload v-if="viewType === 'Upload'"></Upload>

      <div class="formWrap">
        <div class="leftWrap">
          <h4 v-if="left > 0">Left branch</h4>
          <ElementForm @element-values='handleLeftElementValues($event, i)' class="leftForm" v-for="(n, i) in left" :key="30+n"/>
        </div>
        <div class="middleWrap">
          <h4 v-if="middle > 0">Middle branch</h4>
          <ElementForm @element-values='handleMiddleElementValues($event, i)' class="middleForm" v-for="(n, i) in middle" :key="40+n"/>
        </div>
        <div class="rightWrap">
          <h4 v-if="right > 0">Right branch</h4>
          <ElementForm @element-values='handleRightElementValues($event, i)' class="rightForm" v-for="(n, i) in right" :key="50+n"/>
        </div>
      </div>

      <div class="form-buttons">
        <button v-on:click='showModal' v-if='left>0 || right>0 || middle>0' type='button'>Calculate</button>
      </div>
    </div>

    <Modal v-show="modalVisibility" @close="hideModal" @send-params="sendKreatorData"></Modal>
    <Help v-show="helpVisibility" @close="hideHelp"></Help>

    <div id="chart-panel">
      <Chart :width="850" :height="680"/>
    </div>
    <div id='results'>

    </div>

  </div>
</template>

<script>
import PiType from './components/PiType'
import TType from './components/TType'
import Upload from './components/Upload'
import ElementForm from './components/ElementForm'
import Modal from './components/Modal'
import Chart from './components/Chart'
import Help from './components/Help'

export default {
  name: 'App',
  components: {
    PiType,
    TType,
    Upload,
    ElementForm,
    Modal,
    Chart,
    Help
  },
  data() {
    return {
      viewType: 'Pi',
      left: 0,
      middle: 0,
      right: 0,
      leftElements: [],
      middleElements: [],
      rightElements: [],
      modalVisibility: false,
      helpVisibility: false,
      params: new FormData()
    }
  },

  methods: {
    clearValues() {
      this.left = 0;
      this.middle = 0;
      this.right = 0;
    },

    showModal() {
      this.modalVisibility = true;
    },

    hideModal() {
      this.modalVisibility = false;
    },

    showHelp() {
      this.helpVisibility = true;
    },

    hideHelp() {
      this.helpVisibility = false;
    },

    handleElementQuantities(values) {
      this.left = values.left;
      this.middle = values.middle;
      this.right = values.right;
    },

    handleLeftElementValues(value, index) {
      // tutaj z jakiegoś powodu znika rodzaj elementu
      this.leftElements[index] = value;
    },

    handleMiddleElementValues(value, index) {
      this.middleElements[index] = value;
    },

    handleRightElementValues(value, index) {
      this.rightElements[index] = value;
    },

    sendKreatorData(values) {
      // TODO: HERE CHANGE THE BACKEND URL
      this.$http.post('www.test.com/test', {
        schehmatic: this.viewType,
        elements: {
          left: this.leftElements,
          middle: this.middleElements,
          right: this.rightElements
        },
        parameters: {
          points_count: values['points_count'],
          start_frequency: values['start_frequency'],
          stop_frequency: values['stop_frequency']
        }
      })
        .then(function(response) {
        })
        .catch(function(error) {
        });
    }
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .formWrap {
    margin-left: 205px;
  }

  .nameWrap{
    margin-left: -200px;
    margin-top: -20px;
  }

  .leftWrap {
    float: left;
    margin: 20px;
  }

  .middleWrap {
    float: left;
    margin: 20px;
  }

  .rightWrap {
    float: left;
    margin: 20px;
  }

  .scheme-type {
    width: 85%;
  }

  #circuit-panel {
    margin-top: -20px;
    width: 45%;
    height: 690px;
    text-align: center;
    background: #fff;
    padding: 40px 0px 20px 0px;
    border-radius: 50px;
    float: left;
  }

  #chart-panel {
    margin-top: -20px;
    width: 45%;
    height: 690px;
    text-align: center;
    background: #fff;
    padding: 40px 0px 20px 0px;
    border-radius: 50px;
    float: right;
  }

  .form-buttons{
    clear: both;
    text-align: center;
    margin-right: 30px;
  }
</style>
