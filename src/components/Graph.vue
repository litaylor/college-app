<template>
  <div id="graph">
    <div v-if="tempArray.length>0" v-for="school in tempArray">
      <p>
        {{ school[0] }} {{ school[1] }} - {{ school[2] }}
      </p>
    </div>
  </div>
</template>

<script>
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
