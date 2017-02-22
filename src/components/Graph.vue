<template>
  <div id="graph">
    <!-- Highlight component shows popup when user clicks school -->
    <highlight
      :school="highlightSchool"
      :schoolList="schools"
      :satr="satr"
      :satm="satm"
      :satw="satw"
      :act="act">
    </highlight>
    <!-- Event listener for when user closes popup from Highlight component -->
    <div style="display: none" v-on:cleared="this.drawGraph(this.svg, this.activeField, this.alertFunction)">
    </div>
  </div>
</template>

<script>
  // load highlight
  import highlight from '../components/Highlight'

  // load d3 to create graph
  import * as d3 from 'd3'
  // const data = [99, 71, 78, 25, 36, 92]

  export default {
    name: 'graph',
    components: {
      highlight
    },
    props: [
      'schools',
      'activeField',
      'satr',
      'satm',
      'satw',
      'act',
      'fields',
      'testCode',
      'outsideHighlightSchool'
    ],
    data () {
      return {
        highlightSchool: []
      }
    },
    mounted () {
      this.drawGraph(this.svg, this.activeField, this.alertFunction)
    },
    methods: {
      alertFunction: function (school) {
        this.highlightSchool = school
      },
      drawGraph: function (svg, activeField, alertFunction) {
        // Clear current svg
        svg.selectAll('*').remove()

        // Start creating new drawing on svg
        var scoreData = []
        scoreData.push(this[activeField])

        // Bars
        svg.selectAll('bar')
          .data(this.tempArray)
          .enter().append('rect')
          .on('click', function (d) {
            alertFunction(d)
          })
          .attr('class', function (d) {
            if (d[2] < scoreData[0]) {
              return 'safety'
            } else if (d[1] > scoreData[0]) {
              return 'reach'
            } else {
              return 'target'
            }
          })
          .attr('x', function (d) {
            var xMinScale
            if (activeField === 'act') {
              xMinScale = d3.scaleLinear()
                .domain([0, 36])
                .range([0, 100])
            } else {
              xMinScale = d3.scaleLinear()
                .domain([0, 800])
                .range([0, 100])
            }

            var scoreMin = xMinScale(d[1])
            return scoreMin
          })
          .attr('width', function (d) {
            var xMaxScale
            if (activeField === 'act') {
              xMaxScale = d3.scaleLinear()
                .domain([0, 36])
                .range([0, 100])
            } else {
              xMaxScale = d3.scaleLinear()
                .domain([0, 800])
                .range([0, 100])
            }

            var scoreMin = xMaxScale(d[2] - d[1])
            return scoreMin
          })
          .attr('y', function (d, i) {
            return (i * 100 / 11)
          })
          .attr('height', function (d, i) {
            return (100 / 11 - 2)
          })

        // Add minimum score labels to left side of bars
        svg.selectAll('text.mins')
          .data(this.tempArray)
          .enter().append('text')
          .attr('class', 'label minLabel')
          .attr('text-anchor', 'end')
          .attr('alignment-baseline', 'middle')
          .attr('transform', 'scale(0.4, 1)')
          .attr('x', function (d, i) {
            var xMinScale
            if (activeField === 'act') {
              xMinScale = d3.scaleLinear()
                .domain([0, 36])
                .range([0, 100])
            } else {
              xMinScale = d3.scaleLinear()
                .domain([0, 800])
                .range([0, 100])
            }
            var labelMin = xMinScale(d[1]) - 1
            return (labelMin / 0.4)
          })
          .attr('y', function (d, i) {
            return (i * 100 / 11 + 0.5 * (100 / 11))
          })
          .text(function (d) {
            return d[1]
          })

        // Add maximum score labels to right side of bars
        svg.selectAll('text.maxs')
          .data(this.tempArray)
          .enter().append('text')
          .attr('class', 'label maxLabel')
          .attr('text-anchor', 'start')
          .attr('alignment-baseline', 'middle')
          .attr('transform', 'scale(0.4, 1)')
          .attr('x', function (d, i) {
            var xMinScale
            if (activeField === 'act') {
              xMinScale = d3.scaleLinear()
                .domain([0, 36])
                .range([0, 100])
            } else {
              xMinScale = d3.scaleLinear()
                .domain([0, 800])
                .range([0, 100])
            }
            var labelMin = xMinScale(d[2]) + 1
            return (labelMin / 0.4)
          })
          .attr('y', function (d, i) {
            return (i * 100 / 11 + 0.5 * (100 / 11))
          })
          .text(function (d) {
            return d[2]
          })

        // Add vertical My Score line
        svg.selectAll('line')
          .data(scoreData)
          .enter().append('line')
          .attr('class', 'scoreLine')
          .attr('x1', function (d) {
            var lineScale
            if (activeField === 'act') {
              lineScale = d3.scaleLinear()
                .domain([0, 36])
                .range([0, 100])
            } else {
              lineScale = d3.scaleLinear()
                .domain([0, 800])
                .range([0, 100])
            }
            return lineScale(d)
          })
          .attr('y1', 0)
          .attr('x2', function (d) {
            var lineScale
            if (activeField === 'act') {
              lineScale = d3.scaleLinear()
                .domain([0, 36])
                .range([0, 100])
            } else {
              lineScale = d3.scaleLinear()
                .domain([0, 800])
                .range([0, 100])
            }
            return lineScale(d)
          })
          .attr('y2', 100)

        // Add label on top of My Score line
        svg.selectAll('text.scoreLabel')
          .data(scoreData)
          .enter().append('text')
          .attr('class', 'label scoreLabel')
          .attr('text-anchor', 'middle')
          .attr('alignment-baseline', 'middle')
          .attr('transform', 'scale(0.4, 1)')
          .attr('x', function (d) {
            var lineScale
            if (activeField === 'act') {
              lineScale = d3.scaleLinear()
                .domain([0, 36])
                .range([0, 100])
            } else {
              lineScale = d3.scaleLinear()
                .domain([0, 800])
                .range([0, 100])
            }
            return (lineScale(d) / 0.4)
          })
          .attr('y', -3)
          .text('My score')

        // Add school name labels on left side of graph
        svg.selectAll('text.schoolLabel')
          .data(this.tempArray)
          .enter().append('text')
          .on('click', function (d) {
            alertFunction(d)
          })
          .attr('class', function (d) {
            if (d[2] < scoreData[0]) {
              return 'label schoolLabel safety'
            } else if (d[1] > scoreData[0]) {
              return 'label schoolLabel reach'
            } else {
              return 'label schoolLabel target'
            }
          })
          .attr('text-anchor', 'left')
          .attr('alignment-baseline', 'middle')
          .attr('transform', 'scale(0.4, 1)')
          .attr('x', 0)
          .attr('y', function (d, i) {
            return (i * 100 / 11 + 0.5 * (100 / 11))
          })
          .text(function (d) {
            return d[0]
          })

        return 'Graph'
      }
    },
    computed: {
      // Create svg
      svg () {
        return d3.select(this.$el)
          .append('svg')
          .attr('class', 'graphCanvas')
          .attr('viewBox', '-5 -5 110 110')
          .attr('preserveAspectRatio', 'none')
      },
      // Create test scores array based on selected test
      tempArray () {
        var myData = this.schools.schools
        var i
        var helperArray = []
        var testCode = this.testCode
        for (i in myData) {
          if (this.activeField !== 'act') {
            helperArray.push([myData[i].name, myData[i].sat[testCode].min, myData[i].sat[testCode].max])
          }
          if (this.activeField === 'act') {
            helperArray.push([myData[i].name, myData[i].act.min, myData[i].act.max])
          }
        }
        return helperArray
      }
    },
    watch: {
      outsideHighlightSchool: function () {
        this.highlightSchool = this.outsideHighlightSchool
      },
      // These all make sure to redraw the graph immediately if the user changes the test or any of their scores
      tempArray: function () {
        this.drawGraph(this.svg, this.activeField, this.alertFunction)
      },
      satr: function () {
        this.drawGraph(this.svg, this.activeField, this.alertFunction)
      },
      satm: function () {
        this.drawGraph(this.svg, this.activeField, this.alertFunction)
      },
      satw: function () {
        this.drawGraph(this.svg, this.activeField, this.alertFunction)
      },
      act: function () {
        this.drawGraph(this.svg, this.activeField, this.alertFunction)
      }
    }
  }
</script>

<style>
  #graph {
  }
  .graphCanvas {
    width:720px;
    height: 240px;
  }
  rect.safety, rect.reach {
    fill: #644D52;
  }
  rect.target {
    fill: #F77A52;
  }
  rect:hover{
    stroke-opacity:0.4;
    stroke-width:3;
    stroke-linejoin: round;
  }
  rect.target:hover{
    stroke:#F77A52;
  }
  rect.safety:hover, rect.reach:hover{
    stroke:#644D52;
  }
  .label {
    font-size: 4px;
    font-family: 'Lato';
  }
  .schoolLabel {
    font-size: 5px;
  }
  .schoolLabel:hover {
    font-weight: bold;
  }
  @media (max-width: 660px) {
    .label:not(.scoreLabel) {
      transform: scale(0.65,1)

    }
  }
  .schoolLabel.target {
    text-decoration: underline;
    fill: #FF974F;
  }
  .scoreLine {
    stroke: #644D52;
    stroke-width: 0.5;
    opacity: 0.8;
  }
  @media (max-width:768px) {
    .graphCanvas {
      width:100%;
    }
    .maxLabel, .minLabel, .scoreLabel {
      display: none
    }
  }
  .schoolLabel, rect.target, rect.safety, rect.reach {
    cursor: pointer;
  }
</style>
