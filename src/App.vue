<template>
  <div id="app">
    hey {{ message }}
    <schoolInfo :schoolInfo="schools">

    </schoolInfo>
  </div>
</template>

<script>
import axios from 'axios'
import schoolInfo from './components/schoolInfo'
export default {
  name: 'app',
  components: {
    schoolInfo
  },
  data () {
    return {
      message: 'this the message',
      schools: []
    }
  },
  mounted () {
    axios.get('/static/college-data.json')
    .then((response) => {
      // console.log(response.data)
      this.schools = response.data
    })
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
}
</style>
