<template>
  <div class="darkPadding">
      <label class="toggle">
          <img class="cursor-pointer" :src="(mode === 'dark') ?  Moon : mode = 'light' ? Ice : Sun " @click="$emit('nightMode')"/>
          <!-- d.y >= 70 ? "green" : d.y >= 50 ? "yellow" : "red"; -->
          <!-- <img class="cursor-pointer" :src="(mode === 'dark') ?  Sun  : Moon " @click="$emit('nightMode')"/> -->
      </label>
  </div>
</template>

<script>
import Sun from '../assets/images/icon-sun.svg'
import Moon from '../assets/images/icon-moon.svg'
import Ice from '../assets/images/icon-delete.svg'
export default {
    props: ['mode'],
    name: 'DarkModeButton',
    data() {
        return{
            Sun,
            Moon,
            Ice
        }
    },

    // methods: {
    //     nightMode() {
    //         if(this.mode === "dark") {
    //             this.mode = Sun
    //         } else if(this.mode === "dark") {
    //             this.mode = Moon
    //         }
    //     }
    // }

}
</script>

<style>
 
</style>