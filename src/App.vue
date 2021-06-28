<template>
  <div>
    <!-- <router-view v-slot="{ Component }">
      <transition name="route" mode="out-in">
        <component :is="Component"></component>
      </transition>
    </router-view> -->
    
      <!-- <transition name="route" mode="out-in"> -->
        <router-view ></router-view>
      <!-- </transition> -->
    
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

.route-enter-from{
  opacity: 0;
  transform: translateX(100px);
}
.route-enter-active {
  transition: all 0.3s ease-out;
}

.route-leave-to {
  opacity: 0;
  transform: translateX(-100px);
}
.route-leave-active {
  transition: all 0.3s ease-in;
}
</style>
