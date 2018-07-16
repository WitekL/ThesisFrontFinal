<template>
  <div id="app">
    <div id='circuit-panel'>
      <div class="scheme-type" @change="clearValues">
        <label>Pi</label><input type="radio" v-model="x" value="Pi">
        <label>T</label><input type="radio" v-model="x" value="T">
        <label>Upload</label><input type="radio" v-model="x" value="Upload">
      </div>
      <PiType v-if="x === 'Pi'" @element-quantities="handleElementQuantities($event)"></PiType>
      <!-- <TType v-else></TType> -->
      <Upload v-if="x === 'Upload'"></Upload>

      <div class="formWrap">
        <div class="nameWrap">
          <label>Circuit name:</label>
          <input name="name" v-model="circuitName"/>
        </div>
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
        <button v-on:click='saveCircuit' v-if='left>0 || right>0 || middle>0' type='button'>Save circuit</button>
        <button v-on:click='showModal' v-if='connectionsNumber === 0 && (left>0 || right>0 || middle>0)' type='button'>Calculate</button>
      </div>
    </div>

    <Modal v-show="modalVisibility" @close="hideModal"></Modal>

    <div id='connections-panel'>

    </div>
    <div id='results'>

    </div>

  </div>
</template>

<script>
import PiType from './components/PiType'
// import TType from './components/TType'
import Upload from './components/Upload'
import ElementForm from './components/ElementForm'
import Modal from './components/Modal'

export default {
  name: 'App',
  components: {
    PiType,
    // TType
    Upload,
    ElementForm,
    Modal
  },
  data() {
    return {
      x: 'Pi',
      left: 0,
      middle: 0,
      right: 0,
      leftElements: [],
      middleElements: [],
      rightElements: [],
      savedCircuit: [],
      circuits: {},
      circuitName: '',
      connectionsNumber: 0,
      connections: {},
      modalVisibility: false
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

    handleElementQuantities(values) {
      this.left = values.left;
      this.middle = values.middle;
      this.right = values.right;
    },

    handleLeftElementValues(value, index) {
      this.leftElements[index] = value;
    },

    handleMiddleElementValues(value, index) {
      this.middleElements[index] = value;
    },

    handleRightElementValues(value, index) {
      this.rightElements[index] = value;
    },

    saveCircuit() {
      this.savedCircuit.push(this.leftElements, this.middleElements, this.rightElements);
      this.circuits[this.circuitName] = this.savedCircuit;

      this.savedCircuit = {};
      this.left = 0;
      this.middle = 0;
      this.right = 0;

      this.connectionsNumber++
    },

    sendSingleNetwork() {
      this.$http.post('www.test.com/test', {
        elements: {
          left: this.leftElements,
          middle: this.middleElements,
          right: this.rightElements
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
    margin-left: 70px;
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
    /*position: absolute;*/
    /*left: 360px;*/
    /*top: 25px;*/
    width: 85%;
  }

  #circuit-panel {
    margin-top: -20px;
    width: 50%;
    height: 690px;
    text-align: center;
    background: #fff;
    padding: 40px 0px 20px 0px;
    border-radius: 50px;
  }

  .form-buttons{
    clear: both;
    text-align: center;
    margin-right: 30px;
  }
</style>
