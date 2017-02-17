<template lang="html">
  <div id="myStats" class="scorePanel myScores">
    <h3> My scores </h3>
    <ul>
      <li> {{ satr }} </li>
      <li> {{ satm }} </li>
      <li> {{ satw }} </li>
    </ul>
    <h3> &nbsp; </h3>
  </div>
</template>

<script>
export default {
  name: 'myStats',
  props: [
    'satr',
    'satm',
    'satw',
    'act'
  ],
  data () {

  }
}
</script>

<style lang="css">
</style>
