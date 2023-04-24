<template>
  <!-- Navegação das Páginas -->
  <div id="nav">
    <router-link to="/" id="logo_url">
      <img :src="logo" :alt="alt" id="logo">
    </router-link>
    <router-link to="/">Home</router-link>
    <router-link to="/pedidos">Pedidos</router-link>
  </div>
</template>

<script>
export default{
  name: 'NavBar',
  props:{
    logo:String,
    alt:String
  }
}
</script>

<style scoped>

#nav{
  background-color: #222;
  border-bottom: 4px solid #111;
  padding: 15px 50px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

#nav #logo_url{
  margin: auto;
  margin-left: 0;
}

#logo{
  width: 40px;
  height:40px;
}

#nav a {
  color: #FCBA03;
  text-decoration: none;
  margin: 12px;
  transition: .5s;
}

#nav a:hover{
  color: white;
}
</style>