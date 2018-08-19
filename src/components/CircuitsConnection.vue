<template>
  <div>
    <select>
      <option value = "1">1</option>
      <option value = "2">2</option>
      <option value = "3">3</option>
    </select>
    <select>
      <option value = "series">series</option>
      <option value = "parallel">parallel</option>
      <option value = "chain">chain</option>
    </select>
    <select>
      <option value = "1">1</option>
      <option value = "2">2</option>
      <option value = "3">3</option>
    </select>
    <select>
      <option value = "series">series</option>
      <option value = "parallel">parallel</option>
      <option value = "chain">chain</option>
    </select>
    <select>
      <option value = "1">1</option>
      <option value = "2">2</option>
      <option value = "3">3</option>
    </select>
  </div>
</template>


<script>

  export default {
    name: 'CircuitsConnection',

    data() {
      return {
        circuits: this.$parent.circuits
      }
    }
  }

</script>
