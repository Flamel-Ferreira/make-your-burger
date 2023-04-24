<template>
 <div id="main_banner">
  <h1>Make Your Burger</h1>
 </div>
</template>

<script>
export default{

}
</script>
<style scoped>
  #main_banner{
    background-image:url('../../public/img/burger.jpg');
    background-size: cover;
    background-position: 0 -250px;
    height: 500px;
    display:flex;
    align-items: center;
    justify-content: flex-start;
  }

  #main_banner h1{
    color: white;
    text-align: center;
    font-size: 60px;
    background-color: #222;
    padding: 20px 40px;
  }
</style>