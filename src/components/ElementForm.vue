<template>
  <form>
    <select name="Type" v-model="type">
      <option value="r">R</option>
      <option value="c">C</option>
      <option value="l">L</option>
    </select>
    <input v-on:change="emitValues" v-bind:class="type" type="text" name="Value" v-model.number="value" size="4">
    <span class="unit">{{ this.unit }}</span>
  </form>
</template>


<script>

  export default {
    name: 'Form',

    data() {
      return {
        type: '',
        value: ''
      }
    },

    methods: {
      emitValues() {
        var element = {};
        element.type = this.type;
        element.value = this.value;
        this.$emit('element-values', element);
      }
    },

    computed: {
      unit: function() {
        if(this.type === 'c') {
          return 'pF' 
        }
        else if (this.type === 'l') {
          return 'nH'
        }
        else {
          return ''
        }
      }
    }
  }
</script>

<style>
  .unit {
    position: absolute;
    margin-left: 5px;
  }
</style>
