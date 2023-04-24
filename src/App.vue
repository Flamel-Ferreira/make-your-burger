<template>
  <!-- Cabeçalho-->
  <NavBarComponent :logo="logo_src" :alt="app_name"/> 
  <!--Local onde será renderizado as Views-->
  <router-view/>
  <!-- Rodapé -->
  <FooterComponent/>
</template>

<script>
import NavBarComponent from './components/NavBarComponent.vue'
import FooterComponent from './components/FooterComponent.vue'
export default{
  components:{
    NavBarComponent,
    FooterComponent
  },
  data(){
    return{
      logo_src: "/img/logo.png",
      app_name: "Make your Burguer"
    }
  }
}
</script>

<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family: Helvetica;
}

.main_container{
  margin:50px;
  min-height: 250px;
}

h1{
  text-align:center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
