<template>
  <div>
        <div v-for="(addItem, counter) in addItems" :key="counter" class="flex justify-between items-center gap-4 pb-4">
            <div>
                <label class="text-sm">Item Name</label>
                <input required type="text" class="input-group w-4/12" v-model="addItem.itemname ">
            </div>
           <div>
                <label class="text-sm">Qty</label>
                <input required type="number" class="input-group w-2/12" v-model="addItem.quantity">
            </div>
            <div>
                <label class="text-sm">Price</label>
                <input required type="number" class="input-group w-2/12" v-model="addItem.price">
            </div>
            
            <div class="w-2/12">
                <label class="text-sm">Total</label>
                <input :value="addItem.quantity*addItem.price" disabled class="px-1 py-2 focus:outline-none border-none w-9/12" >
            </div>
      
            <div class="w-2/12 cursor-pointer" @click="deleteItem(counter)"><img class="mx-auto mt-5" src="../assets/images/icon-delete.svg" alt="plus-icon" /></div>

        </div>

    <button  @click.prevent="AddItem" class="mt-6 add-button focus:outline-none w-full text-white rounded-2xl my-3 text-xs font-bold py-3 text-center "> + Add Items</button>

  </div>
</template>

<script>
export default {
    props:['addItems'],
    name: 'AddItem',
    components: {

    },

    data() {
        return {
        
        }
    },

    methods:{

      AddItem() {
        this.addItem = ({})
        this.addItems.push(this.addItem)
      },

      deleteItem(counter){
        this.addItems.splice(counter,1);
      },

    },  
   


}

</script>

<style>
.add-button{
    background: #f1f4fd;
    color:#7c5dfa;
     transition: 0.3s linear all;
}
.add-button:hover{
    background: #ced7f7;
    color:#7c5dfa;
     transition: 0.3s linear all;
}

</style>