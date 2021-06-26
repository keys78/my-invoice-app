<template>
  <div class="my-iv-hold h-screen">
      <div v-if="showModal">
        <Modal @close="toggleModal"/>
      </div>

      <div class="xl:w-6/12 lg:w-8/12 md:w-9/12 w-11/12 mx-auto pt-16">
        <div class="flex justify-between items-center pb-10">

            <div class="titleHolder w-4/12">
                <h1 class="sm:text-3xl text-xl font-bold pb-1 tracking-wide">Invoices</h1>
                <p class="sm:text-sm text-xs">There are total of <span class="text-semibold">{{ invoices.length}}</span> Invoice(s)</p>
            </div>

            <!-- <div class="statusHolder sm:w-3/12 w-2/12">

                <div @click="statusToggle" class="">
                    <div v-if="!statusBar" class="flex sm:gap-4 gap-1 items-center cursor-pointer">
                        <p class="sm:block hidden sm:text-sm text-xs font-bold">Filter by status</p>
                        <p class="sm:hidden block sm:text-sm text-xs font-bold">Filter</p>
                        <img class="mx-auto" src="./assets/images/icon-arrow-down.svg" alt="sideArrow" />
                    </div>
                    <div v-if="statusBar" class="flex sm:gap-4 gap-1 items-center cursor-pointer">
                        <p class="sm:block hidden sm:text-sm text-xs font-bold">Filter by status</p>
                        <p class="sm:hidden block sm:text-sm text-xs font-bold">Filter</p>
                        <img class="mx-auto" src="./assets/images/icon-arrow-right.svg" alt="sideArrow" />
                    </div>
                </div>

                <div v-if="statusBar" class="relative ">
                    <div class="absolute rounded sm:pl-6 pl-2 pt-3 statusDrop px-8 shadow spartan">
                            <div>
                                <input  type="checkbox" name="status" v-model="paid"/>
                                <label class="font-bold text-xs pl-3" for="Paid">Paid</label>
                            </div>
                            <div class="py-2">
                                <input type="checkbox" name="status" v-model="unpaid"/>
                                <label class="font-bold text-xs pl-3" for="Pending">Pending</label>
                            </div>
                    </div>
                </div>
                
            </div>-->



            <div @click="toggleModal" class="flex save-button gap-2 px-1 py-2 sm:w-2/12 w-3/12 rounded-3xl items-center justify-center">
                <h1 class="p-2 rounded-full bg-white"><img src="./assets/images/icon-plus.svg" alt="plus-icon" /></h1>
                <Button text="New Invoice" class="sm:block hidden border-none text-white text-xs font-bold"/>
                <Button text="New" class="sm:hidden block border-none text-white text-xs font-bold"/>
            </div>

        </div>

            <div v-if="invoices" class="">
                 <div v-for="invoice in invoices" :key="invoice.id" class="invoice">
                <router-link :to="{name: 'InvoiceDetails', params: { id: invoice.id }}">
                <div class="grid grid-cols-12 items-center md:grid hidden">
                    <h1 class="col-span-2"><span class="text-sm font-bold">#</span><span class="id-style">{{ invoice.id }}</span></h1>
                    <h1 class="col-span-2">Due {{invoice.invoiceDate}}</h1>
                    <h1 class="col-span-3 md:text-center text-right">{{ invoice.clientName }}</h1>
                    <h1 class="col-span-2"> 
                        <span class="flex items-center">
                            <p class="pr-2">&#163;</p>
                            <i class="fas fa-asterisk"></i>
                            <i class="fas fa-asterisk"></i>
                            <i class="fas fa-asterisk"></i>
                            <i class="fas fa-asterisk"></i>
                            <i class="fas fa-asterisk"></i>
                            <i class="fas fa-asterisk"></i>
                        </span>
                    </h1>
                    
                     <div class="my-pending gap-2" :class="{'paid' : invoice.paid}">
                        <h1 class="my-circle h-2 w-2 "></h1>
                        <h1>{{ invoice.statusText }}</h1>
                    </div>
                    <img class="col-span-1 md:block hidden" src="./assets/images/icon-arrow-right.svg" alt="sideArrow" />
                </div>
                </router-link>
            </div>
            </div>
            <div v-else>
                Loading...
            </div>
     
    </div>
  </div>
</template>




<script>
import Modal from './components/Modal.vue'
export default {
    name: 'Invoice',
    components: {
        Modal,
    },

      data () {
        return {
            invoices:[],
            showModal:false,
        }
    },

    beforeMount() {
        this.invoices = JSON.parse(localStorage.getItem('invoices'))
    },

    mounted() {
        this.invoices = JSON.parse(localStorage.getItem('invoices'))
    },
    methods: {
        toggleModal() {
            this.showModal = !this.showModal
        },
    }
}
  




</script>

<style>
.my-pending{
    padding: 8px 1px;
    background: rgba(255, 182, 72, 0.575);
    color: rgb(255, 145, 0);
    width: 110px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
}
.paid{
    padding: 6px 3px;
    background: rgba(123, 241, 87, 0.541);
    color:rgb(4, 139, 26);
}
.paid .my-circle{
    border-radius: 99999px;
    background: rgb(4, 53, 4);
}
.my-circle {
    border-radius: 99999px;
    background: rgb(255, 115, 0);
}


.invoice{
  background: white;
  margin:16px 0px;
  padding: 21px 0px;
  padding-right:10px;
  padding-left:25px;
  font-size: 16px;
  font-weight: 400;
  color:#666ea0;
  border-radius: 5px;
  transition: background 0.5s ease-in-out;
  transition: 0.2s linear;

}
/* .invoice:hover{
 margin-left:-20px;
 transition: linear 0.2s;
} */
.dark .invoice{
  background:#1E2139;
  color:#eff1ff;
  transition: 0.2s linear;
  transition: background 0.5s ease-in-out;
}
.dark .invoice:hover{
 margin-left:-20px;
 transition: linear 0.2s;
}

.myStatu{
  color:rgb(255, 153, 0);
}
.statu{
  color:rgb(20, 184, 20);
}
.id-style{
  font-weight: 700;
}
.statusDrop{
  background: #fff;
}
.dark .statusDrop{
  background: #1E2139;
}
.fa-asterisk{
  font-size: 8px;
}
.my-iv-hold{
    background:rgb(247, 247, 250);
}
</style>