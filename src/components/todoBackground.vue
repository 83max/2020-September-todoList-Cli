<template>
  <div id="app">
    <todoTasklist> </todoTasklist>
  </div>
</template>

<script>
  import todoTasklist from "./todoTasklist";

  export default {
    components:{
      "todoTasklist": todoTasklist

    },

    data () {
      return {

      }
    }
  }
</script>

<style scoped>
  #app{
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    background-image: url(../assets/image1.jpg);
    background-size: cover;
    height: 100vh;
  }

</style>
