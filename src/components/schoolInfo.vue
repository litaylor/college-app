<template>
  <div id="app">
    <div style="margin-top: 40px" v-if="schoolList">

      <graph
      :schools="schools"
      :activeField="activeField"
      :satr="satr"
      :satm="satm"
      :satw="satw"
      :act="act"
      :fields="fields"
      :testCode="testCode">
        it's the graph
      </graph>

      <!-- School Lists -->
      <div v-if="fields==='single'">
        <div v-if="activeField==='satr'">
          SAT Reading
          <div class="schoolCategory">
            <p style = "color:red" class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.sat[testCode].min > satr">
              <p>
                {{ school.name }} <br /> SAT Reading: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:blue" class="header">
              Target schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].min <= satr) && (school.sat[testCode].max >= satr)">
              <p>
                {{ school.name }} <br /> SAT Reading: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:green" class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].max < satr)">
              <p>
                {{ school.name }} <br /> SAT Reading: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
        </div>
        <div v-if="activeField==='satm'">
          SAT Math
          <div class="schoolCategory">
            <p style = "color:red" class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.sat[testCode].min > satm">
              <p>
                {{ school.name }} <br /> SAT Math: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:blue" class="header">
              Target schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].min <= satm) && (school.sat[testCode].max >= satm)">
              <p>
                {{ school.name }} <br /> SAT Math: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:green" class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].max < satm)">
              <p>
                {{ school.name }} <br /> SAT Math: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
        </div>
        <div v-if="activeField==='satw'">
          SAT Writing
          <div class="schoolCategory">
            <p style = "color:red" class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.sat[testCode].min > satw">
              <p>
                {{ school.name }} <br /> SAT Writing: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:blue" class="header">
              Target schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].min <= satw) && (school.sat[testCode].max >= satw)">
              <p>
                {{ school.name }} <br /> SAT Writing: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:green" class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].max < satw)">
              <p>
                {{ school.name }} <br /> SAT Writing: {{ school.sat[testCode].min }} - {{ school.sat[testCode].max }}
              </p>
            </div>
          </div>
        </div>
        <div v-if="activeField==='act'">
          ACT Composite
          <div class="schoolCategory">
            <p style = "color:red" class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.act.min > act">
              <p>
                {{ school.name }} <br /> ACT: {{ school.act.min }} - {{ school.act.max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:blue" class="header">
              Target schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.act.min <= satw) && (school.act.max >= act)">
              <p>
                {{ school.name }} <br /> ACT: {{ school.act.min }} - {{ school.act.max }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:green" class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.act.max < act)">
              <p>
                {{ school.name }} <br /> ACT: {{ school.act.min }} - {{ school.act.max }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import graph from '../components/Graph'

export default {
  name: 'app',
  components: {
    graph
  },
  props: [
    'schools',
    'activeField',
    'satr',
    'satm',
    'satw',
    'act',
    'fields'
  ],
  data () {
    return {
      message: 'this the inside message'
    }
  },
  mounted () {
  },
  computed: {
    schoolList () {
      if (this.schools.length === 0) return false
      return this.schools.schools
    },
    testCode () {
      if (this.activeField === 'satr') return 'reading'
      else if (this.activeField === 'satm') return 'math'
      else if (this.activeField === 'satw') return 'writing'
      else return false
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
}
.schoolCategory {
  display: inline-block;
}
@media(min-width:992px) {
  .schoolCategory {
    box-sizing: border-box;
    width:33%;
    float:left;
  }
}
@media(max-width:991px) {
  .schoolCategory {
    width:100%;
    box-sizing: border-box;
  }
}
.schoolCategory p.header{
  margin-top:60px;
  font-weight: bold;
}
</style>
