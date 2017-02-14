<template>
  <div id="app">
    {{ message }} <br />
    <!-- {{ schoolName }} -->
  </div>
</template>

<script>
export default {
  name: 'app',
  props: [
    'schools'
  ],
  data () {
    return {
      message: 'this the inside message'
      // schoolName: this.schools[0].name
    }
  },
  mounted () {
    console.log(this.schools)
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: blue;
  margin-top: 60px;
  padding:40px;
  border: thin black solid;
}
</style>
