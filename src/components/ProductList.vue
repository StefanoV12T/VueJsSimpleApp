<script setup>
import axios from 'axios';
import { onMounted, ref, computed } from 'vue';

onMounted(() => {
    axios.get(url).then(resp => {
        // console.log(resp.data);
        products.value = resp.data;
    });
});
const productInput = ref(null);

const url = "http://localhost:3000/products";
const products = ref([]);

//computed ref, il computed controlla se il valore di ref cambia e si aggiorna in automatico
// let completedProducts = computed(() => (products.value));
let completedProducts = computed(() => products.value.filter(p => p.completed).length);
let toDoProducts = computed(() => products.value.filter(p => !p.completed).length);




function addProduct(e) {
    // console.log(e.target.value);
    if (e.target.value !="") {
        const dataIn = e.target.value;
        const productIn = {
            name: dataIn,
            completed: false,
        };
        axios.post(url, productIn).then(resp => {
            // console.log(resp.data);
            // products.value = [...products.value, resp.data]; //l'ultimo inserito va in fondo
            products.value = [resp.data, ...products.value,]; //inverto l'ordine di inserimento
        });

        // reset input
        e.target.value = "";
    }
};

function addProductFromIcon() {
    console.log(productInput.value);
    if (productInput.value) {
        addProduct({ target: productInput.value });
    }
}

function deleteProduct(p) {
    axios.delete(`${url}/${p}`).then(resp => {
        products.value = products.value.filter(e=> e.id != p);
    })
}
</script>

<template>
 <!-- sezione per aggiungere un nuovo prodotto -->
<div class="w-full md:w-2/5 mx-auto my-4">
    <label class="input input-bordered flex items-center gap-2">
        <input @keyup.enter="addProduct($event)"  type="text" class="grow" placeholder="Aggiungi Prodotto" ref="productInput" />
        <kbd @click="addProductFromIcon(productInput)" class="kbd kbd-sm">Enter</kbd>
    </label>
</div>

<!-- sezione contatori -->
<div class="w-full md:w-2/5 mx-auto my-4">
   <div class="flex justify-between">
        <button class="btn lg:btn-wide">Completato
            <div class="badge badge-accent">{{completedProducts}}</div>
        </button>
    
        <button class="btn lg:btn-wide">Da Completare
            <div class="badge ">{{toDoProducts}}</div>
        </button>
   </div> 
</div>
  <!-- lista dei prodotti -->

  <div class="w-full md:w-2/5 mx-auto border border-neutral rounded-lg">
    <div v-for="product in products" :key="product.id" class="flex justify-between">
        <!-- icona e nome del prodotto -->
        <div  class="flex items-center"> 
            <button class="btn btn-square btn-ghost" @click="deleteProduct(product.id)">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6 text-error">
                    <path stroke-linecap="round" stroke-linejoin="round" d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
                </svg>
            </button>
            <span :class="{'line-through text-error':product.completed}" class="label-text">{{product.name}}</span>

        </div>
        <!-- checkbox -->
        <label class="label cursor-pointer mr-4">
            <input @change="toogleCompleted(product)" v-model="product.completed" :checked="product.completed" type="checkbox" class="checkbox checkbox-primary" />
        </label>
         
    </div>
  </div>

</template>