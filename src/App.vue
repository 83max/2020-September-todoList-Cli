<template>

  <todoBackground> </todoBackground>

</template>

<script>
  import todoBackground from "./components/todoBackground";

export default {
  name: 'App',
  components: {
    "todoBackground": todoBackground
  }
}
</script>

<style>

  body{
    margin: 0;
    padding: 0;
    text-decoration: none;
    font-family: Avenir,Helvetica,Arial,sans-serif;
    font-size: 20px;
  }

</style>
