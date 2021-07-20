<template>
<div>
    <div class="backdrop" @click.self="$emit('close')">
        <div class="modal w-6/12  animate__animated animate__bounceInLeft">
        <h1 class="pt-12 pb-12 sm:pb-6 md:pl-28 pl-6 mx-auto text-2xl font-semibold">Create Invoice</h1>

        <div class="">
            <form @submit="saveInvoice">
                 <div class="form-holder md:w-9/12 w-11/12 md:pl-6 pl-2 mx-auto overflow-y-scroll">
                    <h2 class="text-sm font-bold mini-headers pb-6">Bill from</h2>

                    <label class="text-sm">Street Address</label>
                    <input v-model="streetAddress" type="text" class="input-group" @blur="handleBlur">

                    <div class="flex justify-between gap-4 mt-5">
                        <div>
                            <label class="text-sm">City</label>
                            <input v-model="city" type="text" class="input-group" @blur="handleBlurCity">
                        </div>
                        <div>
                            <label class="text-sm">Postcode</label>
                            <input v-model="postCode" type="text" class="input-group"  @blur="handleBlurpCode">
                        </div>
                        <div>
                            <label class="text-sm">Country</label>
                            <input v-model="country" type="text" class="input-group" @blur="handleBlurCountry">
                        </div>
                    </div>
                        <!--from-->
                        <h2 class="mt-8 text-sm font-bold mini-headers pb-6">Bill from</h2>

                        <label class="text-sm">Client's Name</label>
                        <input v-model="clientName" type="text" class="input-group"  @blur="handleBlurcName">

                        <div class="py-4">
                            <label class="text-sm pt-4">Client's Email</label>
                            <input v-model="clientEmail" type="text" class="input-group mb-4" @blur="handleBlurcMail">
                        </div>

                        <label class="text-sm">Street Address</label>
                        <input v-model="clientStreetAddress" type="text" class="input-group"  @blur="handleBlurcSA">

                        <div class="flex justify-between gap-4 mt-5">
                            <div>
                                <label class="text-sm">City</label>
                                <input v-model="clientCity" type="text" class="input-group" @blur="handleBlurcCity">
                            </div>
                            <div>
                                <label class="text-sm">Postcode</label>
                                <input v-model="clientPostCode" type="text" class="input-group"  @blur="handleBlurcpCode">
                            </div>
                            <div>
                                <label class="text-sm">Country</label>
                                <input v-model="clientCountry" type="text" class="input-group"  @blur="handleBlurcCountry">
                            </div>
                    </div>

                        <div class="flex justify-between gap-4 mt-5">
                            <div class="w-5/12">
                                <label class="text-sm">Invoice Date</label>
                                <input v-model="invoiceDate" type="date" class="input-group"  @blur="handleBlurivDate">
                            </div>
                            <div  class="w-5/12">
                                <label class="text-sm">Payment Terms</label>
                                <select v-model="paymentTerms" type="text" class="input-group" @blur="handleBlurpTerms">
                                <option value="Net 1 Day">Net 1 Day</option>
                                <option value="Net 7 Days">Net 7 Days</option>
                                <option value="Net 21 Days">Net 21 Days</option>
                                <option value="Net 30 Days">Net 30 Days</option>
                                </select>
                            </div>

                    </div>

                            <div class="py-6">
                                <label class="text-sm">Description</label>
                                <input v-model="description" type="text" class="input-group " placeholder="eg:crypto vendor services" @blur="handleBlurDesc">
                            </div>

                            <h1 class="text-gray-400 font-bold py-5 text-xl">items List</h1>
                            <AddItem :addItems="addItems" />
                            
                </div>      

                    <div class="flex justify-between mt-10 md:w-8/12 w-11/12 mx-auto">
                        <div class="">
                            <button @click.self="$emit('close')" class="discard-button py-4 px-4 text-center rounded-2xl text-xs font-bold focus:outline-none text-white">Discard</button>
                        </div>

                        <div class="flex gap-4">
                            <div>
                                <div @click="saveAsDraft" class="draft-button cursor-pointer py-4 sm:px-4 px-2 text-center rounded-2xl text-xs font-bold focus:outline-none bg-black text-white">Save as Draft</div>
                            </div>

                            <div>
                                <button  :class="{'disable-submit': isDisabled}" type="submit" class="save-button py-4 sm:px-4 px-2 text-center rounded-2xl text-xs font-bold focus:outline-none text-white">Send & Save</button>
                            </div>
                        </div>

                    </div>


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
            invoice: {},
            id: '',
            streetAddress:'',
            city:'',
            postCode: '', 
            country: '', 
            clientName: '', 
            clientEmail: '', 
            clientStreetAddress:'',
            clientCity:'', 
            clientCountry:'', 
            clientPostCode:'', 
            invoiceDate: '', 
            paymentTerms:'',
            description: '', 
            addItems:[{}],
            addItem: {
                itemname: '',
                quantity:'',
                price:'',
            }
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

    methods: {

        saveInvoice() {
            this.invoice = ({
                id: '',
                streetAddress: this.streetAddress,
                city: this.city,
                postCode: this.postCode, 
                country: this.country, 
                clientName: this.clientName, 
                clientEmail: this.clientEmail, 
                clientStreetAddress: this.clientStreetAddress,
                clientCity: this.clientCity, 
                clientCountry: this.clientCountry, 
                clientPostCode: this.clientPostCode, 
                invoiceDate: this.invoiceDate, 
                paymentTerms: this.paymentTerms,
                description: this.description, 
                statusText: 'Pending',
                paid: false,
                showMarkBtn: true,
                draftio: false,
                

                addItems: this.addItems,
                netTotal:this.calcAll
           
                 
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

       

        saveAsDraft() {
            this.invoice = ({
                statusText: "Draft",
                draftio: true
            })
                let uniqueId = ''
                uniqueId += "DRAFT" + String(Math.random()).slice(2, 6);
                this.invoice.id = uniqueId;
            this.saveToLocalStorage();
            this.$router.push({ name: 'InvoiceDetails' })
        },

        pushToLocalStorage() { 
            localStorage.setItem('invoices', JSON.stringify(this.invoices))
        },

        saveToLocalStorage() {
            this.invoices.unshift(this.invoice);
            this.pushToLocalStorage();
        },

         handleBlur(e)  { this.streetAddress.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurCity(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurCountry(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurpCode(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurcName(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurcSA(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurcMail(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurcpCode(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurcCity(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurcCountry(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurivDate(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurpTerms(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' },
         handleBlurDesc(e) { this.city.length > 0 ? e.target.style.border = '1px solid #7c5ef3' : e.target.style.border = 'red 1px solid' }
        
         
    
    },

    computed: {
        // isDisabled() {
        //     return this.streetAddress.length && this.city.length && 
        //     this.postCode.length && this.clientName.length && this.clientEmail.length &&
        //     this.clientCity.length && this.clientPostCode.length && this.invoiceDate.length &&
        //     this.paymentTerms.length && this.description.length > 0 ? false : true
        // },
         calcAll() {
            return this.addItems.reduce((sum, item) => sum + item.price * item.quantity, 0);
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
    z-index: 1;
    animation: switch 0.6s linear;
    
}

@keyframes switch{
    from {
         background: rgba(0,0,0,0.01);
    }
    to{
         background: rgba(0,0,0,0.4);
    }
}

.input-group{
    padding:11px 7px;
    border:1px solid #d9d0ff;
    font-size: 16px;
    width: 100%;
    background: transparent;
    font-family: 'Source Serif Pro', serif;
    border-radius: 7px;
    color:#5d5386;
}
.input-group:focus{
    outline:none;
    border: 1px solid #7c5ef3;
}
::placeholder{
    color:#929596;
}
.mini-headers{
    color:#7c5dfa;

}.form-holder{
    height:70vh;
}

.form-holder::-webkit-scrollbar {
    width: 0.4em;
  }

.form-holder::-webkit-scrollbar-track {
    box-shadow: inset 0 0 2px hsl(192, 100%, 67%)
  }

.form-holder::-webkit-scrollbar-thumb {
    background: #7c5dfa;
    border-radius: 4px;
  }



.save-button{
     background:#7c5dfa;
     transition: 0.3s linear all;
}.save-button:hover{
    opacity: 0.7;
    transition: 0.3s linear all;

}
.draft-button{
     background:#363b53;
     transition: 0.3s linear all;
}
.draft-button:hover {
     background:#000000;
     transition: 0.3s linear all;
}
.discard-button{
    background: #f1f4fd;
    color:#7c5dfa;
     transition: 0.3s linear all;
}
.discard-button:hover{
    background: #ced7f7;
    color:#7c5dfa;
     transition: 0.3s linear all;
}
.delete-button{
    background: rgb(235, 73, 73);
    color:#f5f7ff;
     transition: 0.3s linear all;
}
.delete-button:hover{
    background: #f10e0e;
    color:#f1f4ff;
     transition: 0.3s linear all;
}
.modal{
    background: white;
    color:black;
    transition: background 0.5s ease-in-out;
    border-radius: 20px;
}
 .dark .modal{
     background:rgb(20, 22, 37);
     color:#DFE3FA;
     transition: background 0.5s ease-in-out;
 }
 .dark .input-group{
    background: #1e2139;
    font-family: 'Source Serif Pro', serif;
    color:#DFE3FA;
    border:none;
    transition: background 0.5s ease-in-out;
}

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to{
  transform: translateX(10px);
  opacity: 0;
}

.disable-submit{
    cursor:not-allowed;
}
</style>