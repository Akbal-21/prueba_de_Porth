<script setup lang="ts">
import { computed, ref, watch } from "vue";
import IconRender from "./components/IconRender.vue";
import type { IconsArray } from "./interface";
import { icos_font } from "./ts";
// import icons from "./jsonFormat/icos_font.json";
// * obtencion de todos los iconos
const iconsArray = icos_font;

// * Buscar de iconos 
const search = ref<string>("");
let filteredIcons = ref<IconsArray[]>(iconsArray)

const pageSize:number = 28;
const currentPage = ref<number>(1);

// *observa si cambia el texto el el input search
watch(search, (newValue: string) => {
  const searchLower = search.value.toLowerCase();
  filteredIcons.value = iconsArray.filter(icon => icon.nombre.toLowerCase().includes(searchLower))
  currentPage.value = 1;
})

// * se hacel la paginacion
const paginatedIcons = computed(() => {
  const startIndex = (currentPage.value - 1) * pageSize;
  const endIndex = startIndex + pageSize;
  return filteredIcons.value.slice(startIndex, endIndex);
});

// * se calcula el total de paginacion posible
const totalPages = computed(() => Math.ceil(filteredIcons.value.length / pageSize));

// * se obtiene la pagina siguiente y previa si existe
const previousPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};



</script>

<template>
  <div class="m-3">
  <h2 class="text-center text-5xl italic">Iconos de <span class="underline">fotn awesome</span></h2>
    <div class="m-4">
      <input type="text" placeholder="Busque un icono" v-model="search"
        class="rounded-md w-full h-11 border-blue-500 text-lg" />
    </div>
    <div class="grid sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-3 ">
      <div v-for="(icon, index) in paginatedIcons" :key="index" class="items-center p-3 border-gray-600 bg-slate-300">
        <IconRender :name="icon.nombre" :icon="icon.svg"></IconRender>
      </div>
    </div>
    <div class="text-center m-4">
    <button 
    @click="previousPage" 
    v-show="currentPage != 1"
    :disabled="currentPage === 1" 
    class="text-white bg-gradient-to-r from-cyan-400 via-cyan-500 to-cyan-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-cyan-300 dark:focus:ring-cyan-800 shadow-lg shadow-cyan-500/50 dark:shadow-lg dark:shadow-cyan-800/80 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2"
    >Anterior</button>
    <button 
    @click="nextPage" 
    v-show="currentPage != totalPages"
    :disabled="currentPage === totalPages"
    class="text-white bg-gradient-to-r from-cyan-400 via-cyan-500 to-cyan-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-cyan-300 dark:focus:ring-cyan-800 shadow-lg shadow-cyan-500/50 dark:shadow-lg dark:shadow-cyan-800/80 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2"
    >Siguiente</button>
    </div>

  </div>
</template>

<style scoped></style>
