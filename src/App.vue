<template>
  <div id="app">

    <!-- Header -->

    <myHeader></myHeader>

    <div class="body">
      My
      <div class="menu">
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

        <br />

        <!-- SLIDER INPUT (again, only show one) -->

        <input type="range" step="10" min="200" max="800" v-model.number="satr" v-show="activeField==='satr'" class="slider" />
        <input type="range" step="10" min="200" max="800" v-model.number="satm" v-show="activeField==='satm'" class="slider" />
        <input type="range" step="10" min="200" max="800" v-model.number="satw" v-show="activeField==='satw'" class="slider" />
        <input type="range" step="1" min="1" max="36" v-model.number="act" v-show="activeField==='act'" class="slider" />
      </div>

      <p class="description">
        The bars below represent the middle 50% range of scores of students admitted to each school. Choose your test and enter your score to see how you compare. Data are the most recent available from <a href="https://www.princetonreview.com">The Princeton Review</a>. Not all UNC system schools are listed.
      </p>

      <!-- schoolInfo is the parent of the Graph, MyStats and Highlight components -->

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

    <!-- Footer -->

    <myFooter></myFooter>

  </div>
</template>

<script>

// axios required for AJAX request

import axios from 'axios'

// load components

import myHeader from './components/myHeader'
import myFooter from './components/myFooter'
import schoolInfo from './components/schoolInfo'

// vue instance

export default {
  name: 'app',

  components: {
    schoolInfo,
    myHeader,
    myFooter
  },

  data () {
    return {
      schools: [],
      activeField: 'satr',
      satr: 500,
      satm: 500,
      satw: 500,
      act: 20,
      fields: 'single'
    }
  },
  methods: {
  },
  mounted () {
    // load json file
    axios.get('/static/college-data.json')
    .then((response) => {
      this.schools = response.data
    }

    )
  }
}
</script>

<style>
body {
  margin: 0px;
}
#app {
  font-family: 'Lato';
  color: #644D52;
}
@media (max-width: 768px) {
  .slider {
    width: 50%;
  }
}
@media (min-width: 768px) {
  .slider {
    width: 240px;
  }
}
.fa {
  cursor: pointer;
}
.menu {
  display: inline;
}
.menu select, .menu input {
  padding: 5px;
  margin: 5px;
  border-style: solid;
  border-width: thin;
  border-radius: 5px;
  border-color: #F77A52;
  color: #F77A52;
  font-weight: bold;
}
.description {
  max-width: 720px;
  margin: 0 auto;
  text-align: left;
}
.description {
  padding: 40px 0px;
}
</style>
