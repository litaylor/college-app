<template>
  <div id="graph">
    <!-- <div v-if="tempArray.length>0" v-for="school in tempArray">

    </div> -->
  </div>
</template>

<script>
  import * as d3 from 'd3'
  // const data = [99, 71, 78, 25, 36, 92]

  export default {
    name: 'graph',
    props: [
      'schools',
      'activeField',
      'satr',
      'satm',
      'satw',
      'act',
      'fields',
      'testCode'
    ],
    data () {
      return {
        graphtest: 'hey'
      }
    },
    mounted () {
      this.drawGraph()
    },
    methods: {
      drawGraph: function () {
        // var graphMargin = { top:20, bottom: 20, left: 20, right: 20 }
        const svg = d3.select(this.$el)
          .append('svg')
          .attr('width', '100%')
          .attr('height', '100%')
          .append('g')
          .attr('transform', 'translate(0, 10)')

        svg.selectAll('bar')
          .data(this.tempArray)
          .enter().append('rect')
          .style('fill', 'green')
          .attr('x', 5)
          .attr('width', function (d) {
            return d[1]
          })
          .attr('y', function (d, i) {
            return i * 10
          })
          .attr('height', 7)
      }
    },
    computed: {
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
    }
  }
</script>

<style>
  #graph {
    border: thin red solid;
  }
</style>
