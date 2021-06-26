<template>
<div>
    <div class="my-pending gap-2" :class="{'paid' : invoice.paid}">
        <h1 class="my-circle h-2 w-2 "></h1>
        <h1>{{ invoice.statusText }}</h1>
    </div>

      <div class="bg-green-600 text-white border">
        <li>{{ id }}</li>
        <li>{{ invoice.city }}</li>
        <li>{{ invoice.postCode }}</li>
        <li>{{ invoice.country }}</li>
      </div>

        <div v-for="(addItems, index) in invoice.addItems" :key="index">
            <p class="billTops col-span-3">{{ addItems.itemname }}</p>
            <p class="billTops col-span-2">{{ addItems.quantity }}</p>
            <p class="billTops col-span-2">{{ addItems.price }}</p>
            <p class="billTops col-span-1">&#163;{{ addItems.subTotal = addItems.quantity*addItems.price }}</p>
        </div>

      <button @click="delInv(invoice)">Delete Invoice</button>
      <br>
      <button @click="editInvoice">Edit Invoice</button>
      <br>
      <button @click="markAsPaid">Mark as Paid</button>

        <div v-if="showEdit">
        <div class="backdrop" @click.self="editInvoice">

        <div class="modal w-6/12 bg-red-500 h-screen">
        <h1 class="py-12 pl-6 mx-auto text-2xl font-semibold">Create Invoice</h1>

            <div class="border border-current w-10/12 mx-auto">
                <form @submit.prevent="saveEdit">
                    <div>
                        <label class="text-sm">City</label>
                        <input v-model="city" type="text" class="input-group">
                    </div>
                    <div>
                        <label class="text-sm">Postcode</label>
                        <input v-model="postCode" type="text" class="input-group">
                    </div>
                    <div>
                        <label class="text-sm">Country</label>
                        <input v-model="country" type="text" class="input-group">
                    </div>
                    <input type="submit" value="saveEdit" class="submit">
                </form>
            </div>


        </div>
        </div>
        </div>

</div>
</template>

<script>

export default {
    name: 'InvoiceDetails',
    props: ["id"],
    components: {
     
    },
    data() {
        return{
            city:'',
            postCode:'',
            country:'',
            invoices:[],
            invoice: {},
            showEdit: false,
        }
    },
    
     mounted() {
        this.invoices = JSON.parse(localStorage.getItem('invoices')) 
        this.invoices.forEach(invoice => {
            if(invoice.id === this.id) { this.invoice = invoice }
        });
       
    },

    methods: {
        persistToLocalStorage() {
         localStorage.setItem('invoices', JSON.stringify(this.invoices))
        },

        delInv(invoice) {
            this.invoice = this.invoices.splice(invoice, 1);
            this.persistToLocalStorage()
            this.$router.push({ name: 'Home' })
        },

        editInvoice() {
            this.showEdit = !this.showEdit
            
            this.city = this.invoice.city
            this.postCode = this.invoice.postCode
            this.country = this.invoice.country
        },

        saveEdit() {
            this.invoice.city =  this.city,
            this.invoice.postCode =  this.postCode,
            this.invoice.country =  this.country
            localStorage.setItem('invoices', JSON.stringify(this.invoices))
        },

        markAsPaid() {
            this.invoice.paid = true
            this.invoice.statusText = 'Paid'
            localStorage.setItem('invoices', JSON.stringify(this.invoices))
        }
    },
    
   
}
</script>

<style>
.input-group{
    border:1px solid black;
}

</style>