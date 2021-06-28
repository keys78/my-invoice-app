<template>
 <section :class="mode" >
 <section :class="gift">

   <div class="sidebar fixed left-0 rounded-r-2xl" style="z-index:4;">
      <div class="logo-holder rounded-r-2xl">
        <img class="mx-auto" src="./assets/images/logo.svg" alt="sidepanel" />
      </div>
      <Gifttheme :gift="gift" class="lg:block hidden" @giftMode="giftMode"/>
      <div class="navHolders ">
        <DarkModeButton :mode="mode" class="md:border-b border-r border-gray-700" @nightMode="nightMode"/>
        <div @click="openProfile" class="avatar cursor-pointer">
          <img class="mx-auto rounded-full w-10" src="https://images.pexels.com/photos/371168/pexels-photo-371168.jpeg?crop=faces&fit=crop&h=200&w=200&auto=compress&cs=tinysrgb" alt="sideArrow" />
        </div>
      </div>
    </div>

  <div class="main">
      <Invoice />
  </div>

  <div v-if="showProfile" @click.self="closeProfile" class="mamalet w-full fixed top-0 left-0 h-screen p-10 mx-auto rounded-3xl animate__animated animate__zoomInDown">
      <div class="mt-40 xl:w-4/12 md:w-6/12 sm:w-8/12 w-11/12 mx-auto">
          <img class="mx-auto rounded-full w-40" src="https://images.pexels.com/photos/371168/pexels-photo-371168.jpeg?crop=faces&fit=crop&h=200&w=200&auto=compress&cs=tinysrgb" alt="sideArrow" />
          <h1 class="text-3xl py-4 text-center ">Emcodes</h1>
          <div class="mx-auto text-center sm:w-5/12 w-11/12">
              <div class="flex items-center hover:text-yellow-600 gap-5"><a class="pb-2" href="https://twitter.com/Emmy31087589">Connect with me</a><img class="w-4" src="./assets/images/icon-external-link.svg" alt="link" /></div>
              <div class="flex items-center hover:text-yellow-600 gap-5"><a href="https://github.com/Em-codes/my-invoice-app">Github Repo</a><img class="w-4" src="./assets/images/icon-external-link.svg" alt="link" /></div>
          </div>
      </div>
  </div>
 </section>
 </section>
</template>


<script >
import Invoice from './Invoice.vue'
import DarkModeButton from './components/DarkModeButton.vue'
import Gifttheme from './components/Gifttheme.vue'


  export default {
    name: 'App',
    components: {
      Invoice,
      DarkModeButton,
      Gifttheme

    }, 
    data() {
      return {
        showProfile:false,
        mode:'',
        gift:''
      }
    },

    beforeMount() {
      this.currentmode = localStorage.getItem('mode')
      this.mode = this.currentmode

      this.currentgift = localStorage.getItem('gift')
      this.gift = this.currentgift
    },


    methods: {
      closeProfile() {
      this.showProfile = false
      },

      openProfile() {
      this.showProfile = !this.showProfile
      },

      nightMode() {
       this.mode === 'dark' ? this.mode = 'light' : this.mode = 'dark'
       localStorage.setItem('mode', this.mode)
      },

      giftMode() {
       this.gift === 'red' ? this.gift = 'light' : this.gift = 'red'
       localStorage.setItem('gift', this.gift)
      },
    }

   
  }
</script>


<style >
*{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
    .spartan{
      font-family: 'Spartan', sans-serif;
    }
    .sidebar{
      background: #1E2139;
      width: 100px;
      height:100vh;
    }
    .logo-holder{
      background: #7c5dfa;
      width: 100%;
      padding:35px;
    }
    .navHolders{
      display: flex;
      flex-direction: column;
      position: absolute;
      bottom:40px;
    }
    .avatar{
      padding-top:25px;
      padding-left:20px;
    }
    .darkPadding{
      padding:40px;
    }
    .modal{
      height: 100vh;
    }

    .mamalet{
      background: rgba(0, 0, 0, 0.671);
      color:white;
      
      
    }
    
    .apple{
      background:rgb(250, 247, 248);
      transition: background 0.5s ease-in-out;
    }
    .dark{
      background: rgb(20, 22, 37);
      color:white;
      transition: background 0.5s ease-in-out;
      height:max-content;
    }
   
  .red{
     background-image: url('https://images.pexels.com/photos/698275/pexels-photo-698275.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940');
     background-repeat:no-repeat;
     background-size: cover;
     background-attachment: fixed;
  }
    @media screen and (max-width: 900px) {
   .sidebar{
      width: 100%;
      height: 86px;
      border-radius: 0px;
  
    }
     .logo-holder{
      width: 100px;
      padding:30px;
    }
     .navHolders{
      display: flex;
      flex-direction: row;
      align-items: center;
      position: absolute;
      right:20px;
      bottom: 0px;
    }
    .avatar{
      padding-top:5px;
    }
    .darkPadding{
      padding-bottom:30px;
    }
    .main{
      padding-top: 50px;
    }
    .modal{
      width:100%;
      padding-top:65px;
    }
    .modalEdit{
      padding-top:65px;
      /* width: 100% */
    }
    .form-holder{
    height:60vh;
}
  }

  @media screen and (max-width: 630px) {
    .el-buttono-2{
     margin-top:20px;
     width: 100%;
     display: flex;
     justify-content: space-between;
     align-items: center;
     background:#F9FAFE;
     padding:9px 5px;
   }

   .dark .el-buttono-2{
     background:rgb(3, 1, 31);
   }

  }
  @media screen and (max-width: 590px) {
  .form-holder{
    height:55vh;
}
  .invoice{
    padding-right:10px;
    padding-left:13px;
  }
  }
  


  @media screen and (max-width: 700px) {
    .invoice-holder{
    display: grid;
    justify-content: end;
    align-items: flex-end;
    
    grid-template-columns: repeat(2, 1fr);
    }
    .id-cont{
        grid-column: span 1;
    }
    .date-cont{
        grid-column: span 1;
        text-align: right;
    }
    .name-cont{
        grid-column: span 2;
        
    }
    .total-cont{
        grid-column: span 1;
    }
    .status-cont{
        grid-column: span 1;
        margin:0 auto;
        margin-right: 0px;
    }
   
   
   }
</style>
