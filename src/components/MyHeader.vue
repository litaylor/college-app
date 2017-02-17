<template lang="html">
  <div id="myHeader">
    <h1>College Calculator</h1>
    <h1>University of North Carolina</h1>
    <h3>See how your test scores stack up among the admitted classes at UNC system schools.</h3>
  </div>
</template>

<script>
export default {
}
</script>

<style lang="css">
  #myHeader {
    top:0px;
    left:0px;
    right: 0px;
    background-color: #644D52;
    color: white;
    margin-bottom: 40px;
  }
  @media (max-width: 768px) {
    #myHeader, #myFooter {
      padding: 20px 10px;
    }
    .body {
      padding-left: 10px;
      padding-right: 10px;
    }
  }
  @media (min-width: 769px) {
    #myHeader, #myFooter {
      padding:20px 20px 20px 0px;
      text-align: center;
    }
    h1 {
      display: inline-block;
      padding-left: 40px;
      padding-right: 40px;
      border-right: thin solid white;
    }
    h1:last-of-type {
      border: none;
    }
    h3 {
      padding-left: 40px;
    }
  }
</style>
