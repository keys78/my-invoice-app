<template>
  <div>
      <div v-if="showModal">
        <Modal @close="toggleModal"/>
      </div>

      <div @click="toggleModal" class="flex save-button gap-2 px-1 py-2 sm:w-2/12 w-3/12 rounded-3xl items-center justify-center">
            <h1 class="p-2 rounded-full bg-white"><img src="./assets/images/icon-plus.svg" alt="plus-icon" />Toggle</h1>
        </div>
    <div v-if="invoices" class="w-6/12 mx-auto">
        <ul v-for="invoice in invoices" :key="invoice.id" class="bg-gray-500 text-white border border-black">
            <router-link :to="{name: 'InvoiceDetails', params: { id: invoice.id }}">
                <li>id:{{ invoice.id }}</li>
                <li>city:{{ invoice.city }}</li>
                <li>postcode:{{ invoice.postCode }}</li>
                <li>country:{{ invoice.country }}</li>

                <div class="my-pending gap-2" :class="{'paid' : invoice.paid}">
                    <h1 class="my-circle h-2 w-2 "></h1>
                    <h1>{{ invoice.statusText }}</h1>
                </div>
            </router-link>
        </ul>
    </div>
    <div v-else>
        Loading...
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
            showModal:true,
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
    background: rgb(250, 238, 68);
    color: rgb(255, 2, 2);
    width: 110px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
}
.paid{
    padding: 6px 3px;
    background: rgb(123, 241, 87);
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
</style>