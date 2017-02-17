<template>
  <div id="highlight">
    <div class="fade" v-if="openSwitch==='open'" v-on:click="close()">
      <div class="lightbox">
        <h1> {{ displaySchool.name }} </h1>
        <h3> Acceptance rate: {{ acceptPct }} </h3>
        <h2> Middle 50% test scores: </h2>

        <div class = "scorePanel schoolScores">
          <h3> SAT </h3>
          <ul>
            <li> Reading: {{ displaySchool.sat.reading.min }} - {{ displaySchool.sat.reading.min }} </li>
            <li> Math: {{ displaySchool.sat.math.min }} - {{ displaySchool.sat.math.min }} </li>
            <li> Writing: {{ displaySchool.sat.writing.min }} - {{ displaySchool.sat.writing.min }} </li>
          </ul>

          <h3> ACT </h3>
          <ul>
            <li> Reading: {{ displaySchool.act.min }} - {{ displaySchool.act.min }} </li>
          </ul>
        </div>
        <myStats
          :satr="satr"
          :satm="satm"
          :satw="satw"
          :act="act">

        </myStats>

        <h3> GPA breakdown </h3>
        <h4> Average: {{ displaySchool.gpa.average }} </h4>
        <ul>
          <li> <span class="gradeHeader"> at least 3.75: </span> {{ percenter(displaySchool.gpa.breakdown.min375) }}</li>
          <li> <span class="gradeHeader"> 3.5 - 3.74: </span> {{ percenter(displaySchool.gpa.breakdown.min35) }}</li>
          <li> <span class="gradeHeader"> 3.25 - 3.49: </span> {{ percenter(displaySchool.gpa.breakdown.min325) }}</li>
          <li> <span class="gradeHeader"> 3.00 - 3.24: </span> {{ percenter(displaySchool.gpa.breakdown.min3) }}</li>
          <li v-if="displaySchool.gpa.breakdown.min25 > 0"> <span class="gradeHeader"> 2.5 - 3.0: </span> {{ percenter(displaySchool.gpa.breakdown.min25) }}</li>
          <li v-if="displaySchool.gpa.breakdown.min2 > 0"> <span class="gradeHeader"> 2 - 2.5: </span> {{ percenter(displaySchool.gpa.breakdown.min2) }}</li>
        </ul>

        <button class="close" v-on:click="close()">Close</button>
      </div>
    </div>
  </div>
</template>

<script>
  import * as d3 from 'd3'
  import myStats from '../components/MyStats'

  export default {
    name: 'highlight',
    props: [
      'school',
      'schoolList',
      'satr',
      'satm',
      'satw',
      'act'
    ],
    data () {
      return {
        openSwitch: '',
        saveIndex: -1,
        percenter: d3.format('.0%')
      }
    },
    mounted () {
    },
    methods: {
      assignHighlightSchool: function () {

      },
      close: function () {
        this.openSwitch = ''
      }
    },
    computed: {
      mySchool () {
        if (this.school === {}) {
          return null
        } else {
          var helper = this.school
          return helper
        }
      },
      displaySchool () {
        if (this.saveIndex >= 0) {
          return this.schoolList.schools[this.saveIndex]
        }
      },
      acceptPct () {
        if (this.displaySchool.acceptRate) {
          return this.percenter(this.displaySchool.acceptRate)
        } else {
          return 0
        }
      }
    },
    watch: {
      mySchool: function () {
        if (this.mySchool.length > 0) {
          // var entry
          var name = this.mySchool[0]
          for (var i = 0; i < this.schoolList.schools.length; i++) {
            if (name === this.schoolList.schools[i].name) {
              this.saveIndex = i
            }
          }
          this.openSwitch = 'open'
        }
      }
    },
    components: {
      myStats
    }
  }
</script>

<style>
  .fade {
    position: fixed;
    z-index: 900;
    top:0px; bottom: 0px; left: 0px; right: 0px;
    background-color: rgba(0,0,0,0.7);
  }
  .lightbox {
    position: fixed;
    z-index: 1000;
    top:20%; bottom: 20%; left: 10px; right: 10px;
    background-color: black;
    background-color: #ffffff;
    overflow-y: scroll;
  }
</style>
