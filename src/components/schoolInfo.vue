<template>
  <div id="schoolInfo">
    <div style="margin-top: 40px" v-if="schoolList">

      <!-- Load graph -->

      <graph
        ref="graphRef"
        :schools="schools"
        :activeField="activeField"
        :satr="satr"
        :satm="satm"
        :satw="satw"
        :act="act"
        :fields="fields"
        :testCode="testCode"
        :outsideHighlightSchool="outsideHighlightSchool">
      </graph>

      <p class="description midDescription">
        Click any school for information on its admitted students&rsquo; test scores and average grades.
      </p>

      <!-- School Lists -->
      <div v-if="fields==='single'">

        <!-- List schools if SAT Reading selected -->
        <div v-if="activeField==='satr'" class="categoryWrap">
          <div class="schoolCategory">
            <p class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.sat[testCode].min > satr">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:#F77A52" class="header">
              Target schools <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].min <= satr) && (school.sat[testCode].max >= satr)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].max < satr)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
        </div>

        <!-- List schools if SAT Math selected -->
        <div v-if="activeField==='satm'" class="categoryWrap">
          <div class="schoolCategory">
            <p class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.sat[testCode].min > satm">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:#F77A52" class="header">
              Target schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].min <= satm) && (school.sat[testCode].max >= satm)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].max < satm)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
        </div>

        <!-- List schools if SAT Writing selected -->
        <div v-if="activeField==='satw'" class="categoryWrap">
          <div class="schoolCategory">
            <p class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.sat[testCode].min > satw">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:#F77A52" class="header">
              Target schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].min <= satw) && (school.sat[testCode].max >= satw)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.sat[testCode].max < satw)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
        </div>

        <!-- List schools if ACT selected -->
        <div v-if="activeField==='act'" class="categoryWrap">
          <div class="schoolCategory">
            <p class="header">
              Reach schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="school.act.min > act">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p style = "color:#F77A52" class="header">
              Target schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.act.min <= act) && (school.act.max >= act)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
          <div class="schoolCategory">
            <p class="header">
              Safety schools: <br />
            </p>
            <div
              v-for="school in schoolList" v-if="(school.act.max < act)">
              <p v-on:click="assignOutsideHighlightSchool(school)">
                {{ school.name }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// load graph
import graph from '../components/Graph'

export default {
  name: 'schoolInfo',
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
      // Establish variable allowing clicking on school from this level, pass to Graph -> Highlight to show popup
      outsideHighlightSchool: []
    }
  },
  mounted () {
  },
  methods: {
    assignOutsideHighlightSchool: function (school) {
      this.outsideHighlightSchool = [school.name, 'x', 'x']
      // requires 3-entry-length variable to match clicking from inside Graph
    }
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
  text-align: center;
}
.schoolCategory {
  display: inline-block;
}
.schoolCategory div {
  cursor: pointer;
}
.schoolCategory div:hover p {
  background-color: #F77A52;
  color: white;
}
@media(min-width:992px) {
  .categoryWrap {
    overflow: hidden;
    width: 720px;
    margin: 0 auto;
  }
  .schoolCategory {
    box-sizing: border-box;
    width:33%;
    float:left;
    border-left: thin solid #F77A52;
    padding-bottom:500em;
    margin-bottom:-500em;
  }
  .schoolCategory p.header {
    margin-top:0px;
  }
  .schoolCategory:first-of-type{
    border-left: none;
  }
}
@media(max-width:991px) {
  .schoolCategory {
    width:100%;
    box-sizing: border-box;
  }
  .schoolCategory p.header{
    margin-top:60px;
  }
}
.schoolCategory p.header{
  font-weight: bold;
}
.schoolCategory p{
  -webkit-margin-before: 0;
  -webkit-margin-after: 0;
  line-height: 1.6;
  margin-bottom: 8px;
}
.description a:link, .description a:visited {
  color: inherit;
}
.description a:hover, .description a:active {
  color: #F77A52;
}
</style>
