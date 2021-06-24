<template>
<div>
    <div class="backdrop" @click.self="$emit('close')">
        <div class="modal w-6/12 bg-red-500 h-screen">
        <h1 class="py-12 pl-6 mx-auto text-2xl font-semibold">Create Invoice</h1>

        <div class="border border-current w-10/12 mx-auto">
            <form @submit.prevent="saveInvoice">
                <div>
                    <!-- <input v-model="id"> -->
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
                <AddItem :addItems="invoice.addItems"/>
                <input type="submit" value="submit" class="submit">
            </form>
        </div>


        </div>
    </div>
</div>
</template>

<script>
import AddItem from '../components/AddItem.vue'

export default {
    name: 'Modal',
    components:{
        AddItem
    },
    data() {
        return {
            invoices:[],
            invoice: {
            addItems:[{
                itemname: 'rrdtfhjgkh',
                quantity:'435',
                price:'54657',
                subTotal:'',       
            }],
            },
            id:'',
            city:'',
            postCode:'',
            country:'',
        }
    },

     beforeMount() {
        this.invoices = JSON.parse(localStorage.getItem('invoices'))
    },

    methods: {
        saveInvoice() {
            this.invoice = ({
                 id: '',
                 city: this.city,
                 postCode: this.postCode,
                 country: this.country,
                 statusText: 'Pending',
                 paid: false,

                // addItems: ({
                    // itemname = this.itemname,
                    // quantity = this.quantity,
                    // price = this.price
                // })
                 
            })
       
                let uniqueId = ''
                let letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
                uniqueId += letters.charAt(Math.floor(Math.random() * letters.length))
                uniqueId += letters.charAt(Math.floor(Math.random() * letters.length))
                uniqueId += String(Math.random()).slice(2, 6);
                this.invoice.id = uniqueId;
                console.log(uniqueId)
               
            this.saveToLocalStorage();
           
        },

        pushToLocalStorage() { 
            localStorage.setItem('invoices', JSON.stringify(this.invoices))
        },

        saveToLocalStorage() {
            this.invoices.unshift(this.invoice);
            this.pushToLocalStorage();
        },
    }
}
</script>

<style>
.backdrop{
    width: 100%;
    height: 100%;
    position: fixed;
    top:0;
    background: rgba(0,0,0,0.4);
    
}
.modal{
    background: white;
    color:black;
    transition: background 0.5s ease-in-out;
}
</style>