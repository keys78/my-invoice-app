<template>
  <div class="king">
      <div v-if="showModal" class="elmodali">
        <Modal :mode="mode" @close="toggleModal"/>
      </div>

      <div class="xl:w-6/12 lg:w-8/12 md:w-9/12 w-11/12 mx-auto pt-16">
        <div class="flex justify-between items-center pb-10 animate__animated animate__fadeInDown">

            <div class="titleHolder w-4/12">
                <h1 class="sm:text-3xl text-xl font-bold pb-1 tracking-wide">Invoices</h1>
                <p class="sm:text-sm text-xs">There are total of <span class="text-semibold">{{ invoices.length}}</span> Invoice(s)</p>
            </div>

            <div class="relative sm:w-3/12 w-4/12">
                <div @click="statusToggle">
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



                <div v-if="statusBar" class="absolute w-full">
                    <div class="bg-white status-holder shadow px-3 py-2 animate__animated animate__fadeInDown animate__faster">
                        <div class="flex gap-1">
                            <input  type="checkbox" name="status" v-model="paid" />
                            <label class="font-bold text-xs pl-3" for="Paid">Paid</label>
                        </div>
                        <div class="py-2 flex gap-1">
                            <input type="checkbox" name="status" v-model="pending"/>
                            <label class="font-bold text-xs pl-3" for="Pending">Pending</label>
                        </div>
                        <div class="flex gap-1">
                            <input type="checkbox" name="status" v-model="draft"/>
                            <label class="font-bold text-xs pl-3" for="Pending">Draft</label>
                        </div>
                    </div>
                </div>
            </div>


            <div @click="toggleModal" class="flex save-button gap-2 px-1 py-2 sm:w-2/12 w-3/12 rounded-3xl items-center justify-center cursor-pointer">
                <h1 class="p-2 rounded-full bg-white"><img src="./assets/images/icon-plus.svg" alt="plus-icon" /></h1>
                <Button text="New Invoice" class="sm:block hidden border-none text-white text-xs font-bold"/>
                <Button text="New" class="sm:hidden block border-none text-white text-xs font-bold"/>
            </div>

        </div>

            <div v-if="invoices.length" class="">
                <div v-for="invoice in filteredInvoices" :key="invoice.id" class="invoice animate__animated animate__fadeInLeft">
                    <router-link :to="{name: 'InvoiceDetails', params: { id: invoice.id }}">
                    <div class="invoice-holder">
                        <h1 class="id-cont"><span class="text-sm font-bold">#</span><span class="id-style">{{ invoice.id }}</span></h1>
                        <h1 class="date-cont">Due {{invoice.invoiceDate}}</h1>
                        <h1 class="name-cont">{{ invoice.clientName }}</h1>
                        <!-- <h1 class="total-cont">&#163; {{ invoice.netTotal }}</h1> -->
                        <h1 class="total-cont font-bold" style="font-family: 'Spartan', sans-serif;"> 
                            <!-- <span class="flex items-center font-semibold text-2xl"> -->
                                £{{ invoice.netTotal }}
                                
                            <!-- </span> -->
                        </h1>
                        
                        <div class="flex gap-3 items-center status-cont">
                        <div class="relative" :class="{'draft' : invoice.draftio}">
                            <div class="my-pending py-5" :class="{'paid' : invoice.paid}">
                                <div class="absolute flex gap-2 items-center">
                                    <h1 class="my-circle h-2 w-2 "></h1>
                                    <h1>{{ invoice.statusText }}</h1>
                                </div>
                            </div>
                        </div>
                        <img class="md:block hidden" src="./assets/images/icon-arrow-right.svg" alt="sideArrow" />
                        </div>
                    </div>
                    </router-link>
                </div>
            </div>

            <div v-else>
                <p>Loading Invoice.....</p>
                <img class="h-40 -mt-16" src="https://i.pinimg.com/originals/65/ba/48/65ba488626025cff82f091336fbf94bb.gif" alt="loadre">
                <div v-if="noInvoice" class="text-center">
                    <img class="mx-auto" src="./assets/images/illustration-empty.svg" alt="sideArrow" />
                    <h1 class="font-bold text-xl pt-8">There is nothing here</h1>
                    <p class="invoice-inherit bg-transparent">Create an invoice by clicking the New button and get started.</p>
                </div>
            </div>
    
    </div>
    <p class="pb-40"></p>
  </div>
</template>




<script>
import Modal from './components/Modal.vue'
import Button from './components/Button.vue'
export default {
    name: 'Invoice',
    props: ['mode',],
    components: {
        Modal,
        Button
    },

      data () {
          
        return {
            invoices:[],
            showModal:false,
            paid:false,
            pending:false,
            draft:false,
            statusBar: false,
            noInvoice:false
        }
    },

    beforeMount() {
         this.invoices = JSON.parse(localStorage.getItem('invoices'))

            if (this.invoices === null) {
                fetch('./db.json/') 
                .then(res => { return res.json()})
                .then(data => { localStorage['defaultInvoice'] = JSON.stringify(data)
                this.invoices = JSON.parse(localStorage.getItem('defaultInvoice'))
                localStorage.setItem('invoices', JSON.stringify(this.invoices))
                })
           } else {
                this.invoices = JSON.parse(localStorage.getItem('invoices'))
           }

    },

    mounted(){
        document.title = 'Total Invoices' + ' ' + '('+ this.invoices.length +')'
        this.invoices.length > 0 ? this.noInvoice = false : this.noInvoice = true
    },

    methods: {
        toggleModal() { this.showModal = !this.showModal },

        statusToggle(){ this.statusBar = !this.statusBar },

    },

    computed: {
        filteredInvoices() {
           if(this.paid) {return this.invoices.filter((invoice) => invoice.statusText === "Paid")}
           if(this.pending) { return this.invoices.filter((invoice) => invoice.statusText === "Pending")}
           if(this.draft) {return this.invoices.filter((invoice) => invoice.statusText === "Draft")}
           else return this.invoices
        },  

         sumTotal() {
            //  this.invoices.forEach(invoice => {
            //      return this.$store.getters.invoiceTotal(invoice)
            //  })
            // return 0
         
         },
    }
}
  




</script>

<style>
.my-pending{
    background: rgba(250, 188, 96, 0.13);
    color: #ff5e00;
    font-weight: 500;
    width: 110px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
}
.my-circle {
    border-radius: 99999px;
    background: rgb(255, 115, 0);
}

.draft{
    background: rgba(0, 0, 0, 0.151);
     border-radius: 5px;
}
.draft h1{
    color:rgb(0, 0, 0);
}
.draft .my-circle {
    background: #000;
    border-radius: 99999px;
}
.dark .draft h1{
    color:rgb(246, 251, 255);
}
.dark .draft .my-circle{
    background:rgb(246, 251, 255);
}

.paid{
    background: rgba(10, 219, 45, 0.075);
    color:rgb(8, 214, 42);
}
.paid .my-circle{
    border-radius: 99999px;
    background: rgb(8, 214, 42);
}



.invoice{
  background: white;
  margin:16px 0px;
  padding: 18px 0px;
  padding-right:10px;
  padding-left:25px;
  font-size: 16px;
  font-weight: 400;
  color:#666ea0;
  border-radius: 5px;
  transition: background 0.5s ease-in-out;
  transition: 0.2s linear;

}
.invoice:hover{
 margin-left:-20px;
 transition: linear 0.2s;
}
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
.dark .status-holder {
    background:#0a0e2e;
}

body{
   background:rgb(247, 247, 250);
}




.invoice-holder{
    display: grid;
    grid-template-columns: repeat(11, 1fr);
    align-items: center;
}
.id-cont{
    grid-column: span 2;
}
.date-cont{
    grid-column: span 2;
    margin-left: -20px;
}
.name-cont{
    grid-column: span 3;
}
.total-cont{
    grid-column: span 2;
}
.status-cont{
    grid-column: span 2;
    margin-left: -20px;
}

.king{
    height:200vh;
}

</style>