<script setup>
import { defineProps, defineEmits } from 'vue';

// Definisci le props se necessario
// const props = defineProps(['...']);


//mi permette di richiamare al componente padre una funzione con un determinato valore che paserò con un evento 
const emit = defineEmits(['changeTab']);

</script>

<template>
    <div class="hero min-h-screen">
  <div class="hero-content  flex-col lg:flex-row">
    <img
      src="https://img.daisyui.com/images/stock/photo-1635805737707-575885ab0820.jpg"
      class="max-w-sm rounded-lg shadow-2xl" />
    <div>
      <h1 class="text-5xl font-bold">Lista della memoria</h1>
      <p class="py-6">
        Questa è un'app che ti permette di creare una tua lista, che sia la lista della spesa, gli oridini di lavoro o la lista delle tue allergie
      </p>
      <!-- qui richiamo la funzione da passare al componente padre e il valore che cambia -->
      <button class="btn btn-accent " @click="emit('changeTab', 2);">Accedi alla Lista</button>
    </div>
  </div>
</div>
</template>