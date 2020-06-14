<template>
  <div id="app">
    <Header/>
    <Grid/>
    <Footer/>
  </div>
</template>

<script>

import Header from './components/Header.vue'
import Grid from './components/Grid.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Header,
    Grid,
    Footer,
  },
  created: function() {
    window.document.title = "Tours for Everybody!"
  },
}
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;700&display=swap');
section.section{
  padding:3rem 5rem;
}
section.grid-section{
  padding:0 5rem;
}
@media (max-width:1200px){
 section.section{
    padding:3rem;
  }
}
#app {
  font-family:'Rubik',sans-serif;
  letter-spacing:0px;
  background:#F2F2F2;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  list-style:none;
}
.owl-dots{
  position:absolute;
  bottom:0;  
  background:rgba(0,0,0,0.33);
  width:100%;
  height: 11%;
  align-items: center;
  display: flex;
  justify-content: center;
  border-radius: 0px 0px 20px 20px;
}
</style>
