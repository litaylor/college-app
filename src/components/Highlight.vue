<template>
  <div id="highlight">
    <div class="fade" v-if="openSwitch==='open'">
      <div class="lightbox">
        <h1> {{ displaySchool.name }} </h1>
        <h3 style="color: #F77A52"> Acceptance rate: {{ acceptPct }} </h3>

        <div class="scoresWrap">
          <div class="scoreSection testScores">
            <div class = "scorePanel schoolScores">
              <h3> {{ displaySchool.code.toUpperCase() }} SAT </h3>
              <ul>
                <li> {{ displaySchool.sat.reading.min }} - {{ displaySchool.sat.reading.max }} </li>
                <li> {{ displaySchool.sat.math.min }} - {{ displaySchool.sat.math.max }} </li>
                <li> {{ displaySchool.sat.writing.min }} - {{ displaySchool.sat.writing.max }} </li>
              </ul>

              <h3> {{ displaySchool.code.toUpperCase() }} ACT </h3>
              <ul>
                <li> {{ displaySchool.act.min }} - {{ displaySchool.act.max }} </li>
              </ul>
            </div>
            <div class="centerPanel">
              <h3>&nbsp;</h3>
              <ul>
                <li>Reading</li>
                <li>Math</li>
                <li>Writing</li>
              </ul>
            </div>
            <myStats
              :satr="satr"
              :satm="satm"
              :satw="satw"
              :act="act">

            </myStats>
          </div>

          <div class="scoreSection gpaScores">
            <h3> GPA breakdown </h3>
            <h4> Average: {{ displaySchool.gpa.average }} </h4>
            <ul>
              <li> <span class="gradeHeader"> at least 3.75: </span>
                <span class="gradeNumber"> {{ percenter(displaySchool.gpa.breakdown.min375) }} </span>
              </li>
              <li>
                <span class="gradeHeader"> 3.5 - 3.74: </span>
                <span class="gradeNumber"> {{ percenter(displaySchool.gpa.breakdown.min35) }} </span>
               </li>
              <li>
                <span class="gradeHeader"> 3.25 - 3.49: </span>
                <span class="gradeNumber"> {{ percenter(displaySchool.gpa.breakdown.min325) }} </span>
              </li>
              <li>
                <span class="gradeHeader"> 3.00 - 3.24: </span>
                <span class="gradeNumber"> {{ percenter(displaySchool.gpa.breakdown.min3) }} </span>
                </li>
              <li v-if="displaySchool.gpa.breakdown.min25 > 0">
                <span class="gradeHeader"> 2.5 - 3.0: </span>
                <span class="gradeNumber"> {{ percenter(displaySchool.gpa.breakdown.min25) }} </span>
              </li>
              <li v-if="displaySchool.gpa.breakdown.min2 > 0">
                <span class="gradeHeader"> 2 - 2.5: </span>
                <span class="gradeNumber"> {{ percenter(displaySchool.gpa.breakdown.min2) }} </span>
              </li>
            </ul>
          </div>
        </div>

        <!-- <button class="close" v-on:click="close()">Close</button> -->
        <i v-on:click="close()" class="fa fa-window-close" aria-hidden="true"></i>
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
  .lightbox h1 {
    padding: 20px 10px 10px;
  }
  ul {
    padding: 0;
    list-style-type: none;
  }
  .scorePanel {
    width: 40%;
    float: left;
  }
  .centerPanel {
    width: 20%;
    float: left;
  }
  .scorePanel h3 {
    /*width: 200px;*/
    text-align: center;
  }
  .scoreSection {
    float: left;
  }
  @media (min-width: 601px) {
    .testScores {
      width: 70%;
    }
    .gpaScores {
      width:30%;
    }
  }
  .scoresWrap {
    max-width: 600px;
    margin: 0 auto;
  }
  @media (max-width: 600px) {
    .testScores {
      display: block;
      width: 100%;
    }
    .gpaScores {
      display: block;
      width:100%;
    }
  }
  .fa-window-close {
    position: fixed;
    top: 21%;
    right: 30px;
    font-size: 40px;
    z-index: 2000;
  }
  .lightbox span {
    display: inline-block;
    text-align: center;
  }
  .gradeHeader {
    width: 100px;
  }
  .gradeNumber {
    width: 50px;
  }
</style>
