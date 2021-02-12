<template>
  <div id="app">
      <img alt="Conheça sua sorte" 
	   width="400px"
	   src="./assets/genio_mistico.svg"/>
    <HelloWorld msg="Horóscopo do Místico H." />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
