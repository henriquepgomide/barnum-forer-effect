<template>
  <div id="app">
      <img alt="Conheça sua sorte" 
	   width="400px"
	   src="./assets/genio_mistico.svg"/>
    <Horoscope msg="Horóscopo do Místico HP" />
    <Footer />
  </div>
</template>

<script>
import Horoscope from "./components/Horoscope.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    Horoscope,
    Footer
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
