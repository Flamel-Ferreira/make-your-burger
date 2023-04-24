<template>
  <BannerComponent />
  <div class="main_container">
    <h1>Monte o seu Burger</h1>
    <BurgerFormComponent/>
  </div>
</template>

<script>
import BurgerFormComponent from '@/components/BurgerFormComponent.vue'
import BannerComponent from '../components/BannerComponent.vue'


export default {
  name: 'HomeView',
  components: {
    BannerComponent,
    BurgerFormComponent
  }
}
</script>
