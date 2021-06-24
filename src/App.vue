<template>
  <div>
    <router-view/>
  </div>
</template>
 


 <script>
import Home from './Home.vue'

  export default {
    name: 'App',
    components: {
      Home
    },
  }
 </script>

<style>
*{
  padding: 0;
  margin: 0;
  box-shadow: border-box;
}
</style>
