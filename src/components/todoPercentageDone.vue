<template>

  <div class="div-count">
    <div class="line-through-count" v-bind:class=" {lineThroughShow: isLineThroughShow } "></div>
    <div class="count"> {{completedTodos.length}} / {{remainingTodos.length}}
      (<span
        v-bind:class="
        `${percentageCalcColor === 0 ? 'percentageBlack' :
        percentageCalcColor === 100 ? 'percentageGreen' :
        percentageCalcColor >= 50 ? 'percentageOrange' :
        percentageCalcColor < 50 ? 'percentageRed' : 'percentageBlack'}`"
      >{{percentageCalcColor}}</span>% done)</div>
  </div>

</template>

<script>


  export default {

    props: {
      remainingTodos: {
        type: Array,
        required: false
      },
      completedTodos: {
        type: Array,
        required: false
      },
      percentageCalcColor: {
        type: Number,
        required: false
      },
      isLineThroughShow: {
        type: Boolean,
        required: true
      },
    },
    data () {
      return {
      }
    },

  }
</script>


<style scoped>

  .div-count{
    display: flex;
    padding-bottom: 15px;

  }

  .line-through-count{
    flex-grow: 1;
    border-bottom: 1px solid #ebeae8;
    margin-bottom: 10px;
    margin-right: 10px;
  }

  .lineThroughShow{
    border-bottom: 1px solid #eee0;
  }
  .percentageGreen{
    color: green;
  }

  .percentageOrange{
    color: orange;
  }

  .percentageRed{
    color: red;
  }

  .percentageBlack{
    color: black;
  }

</style>
