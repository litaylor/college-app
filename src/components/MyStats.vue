<template lang="html">
  <div id="myStats" class="scorePanel myScores">
    <h3> My SAT </h3>
    <ul>
      <li> {{ satr }} </li>
      <li> {{ satm }} </li>
      <li> {{ satw }} </li>
    </ul>
    <h3> My ACT </h3>
    <ul>
      <li> {{ act }} </li>
    </ul>
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
    return {
    }
  }
}
</script>

<style lang="css">
</style>
