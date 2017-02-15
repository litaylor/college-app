<template>
  <div id="app">
    My
    <!-- DROPDOWN TO CHOOSE TEST -->
    <select v-model="activeField" class="score" name="score1">
      <option value="satr">SAT Reading</option>
      <option value="satm">SAT Math</option>
      <option value="satw">SAT Writing</option>
      <option value="act">ACT</option>
    </select> score:

    <!-- INPUT TO ENTER SCORE (only show one) -->

    <input size="3" type="number" step="10" min="200" max="800" v-model.number="satr" v-show="activeField==='satr'" />
    <input size="3" type="number" step="10" min="200" max="800" v-model.number="satm" v-show="activeField==='satm'" />
    <input size="3" type="number" step="10" min="200" max="800" v-model.number="satw" v-show="activeField==='satw'" />
    <input size="3" type="number" step="1" min="1" max="36" v-model.number="act" v-show="activeField==='act'" />


    <schoolInfo
    :schools="schools"
    :activeField="activeField"
    :satr="satr"
    :satm="satm"
    :satw="satw"
    :act="act"
    :fields="fields"
    >
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
      schools: [],
      activeField: 'satr',
      satr: 500,
      satm: 500,
      satw: 500,
      act: 20,
      fields: 'single'
    }
  },

  mounted () {
    this.message = 'the new message'

    axios.get('/static/college-data.json')
    .then((response) => {
      this.schools = response.data
    }

    )
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
