<template>
  <div class="darkPadding">
      <label class="toggle">
          <img class="cursor-pointer" :src="(gift === 'red') ?  Smile  : Box " @click="$emit('giftMode')"/>
      </label>
  </div>
</template>

<script>
import Smile from '../assets/images/smile.png'
import Box from '../assets/images/my-gift.png'

export default {
    props: ['gift'],
    name: 'Gifttheme',
    data() {
        return{
            Smile,
           Box
        }
    }

}
</script>

<style>
 
</style>