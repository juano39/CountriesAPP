<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import PageHeader from './components/PageHeader.vue'
import axiosClient from './utils/axis'
import { Country } from './models/country.model';
import CountryList from './components/CountryList.vue'


const ITEMS_PER_PAGE = 8;

const countries = ref<Country[]>([]);
const search = ref("");
const filterCountry = ref<Country[]>([])
const page = ref(1)
const paginatedCountries = ref<Country[]>([])
const totalItems = ref(0)

const fetchCountries = async () => {
  try {
    const { data } = await axiosClient.get("/all")
    countries.value = data
    totalItems.value = countries.value.length
    // Inicialmente, muestra todos los países
    paginatedCountries.value = countries.value.slice(0, ITEMS_PER_PAGE);
  } catch (error) {
    console.error(error)
  }
}

const sliceCountries = () => {
  const start = (page.value - 1) * ITEMS_PER_PAGE;
  const end = page.value * ITEMS_PER_PAGE;
  // Si hay una búsqueda, aplica la paginación a los resultados filtrados
  paginatedCountries.value = filterCountry.value.length <= 0
    ? countries.value.slice(start, end)
    : filterCountry.value.slice(start, end);
}

const changePage = (newPage: number) => {
  page.value = newPage;
}

const filterCountries = () => {
  filterCountry.value = countries.value.filter((country) => country.name.common.toLowerCase().includes(search.value.toLowerCase()))
  // Si hay una búsqueda, reinicia la página a 1 y aplica la paginación a los resultados filtrados
  if (search.value !== '') {
    page.value = 1;
    sliceCountries();
  } else {
    // Si no hay búsqueda, muestra todos los países
    paginatedCountries.value = countries.value.slice(0, ITEMS_PER_PAGE);
  }
}

onMounted(() => {
  fetchCountries();
})

watch([countries, page, filterCountry], () => {
  sliceCountries();
})
</script>

<template>
  <PageHeader />

  <div class="container  mx-auto px-6">
    <div class="mb-8">
      <input type="text" class="border font-bold px-4 border-gray-300 rounded w-full p-1"
        placeholder="Search by country name" @input="filterCountries" v-model="search">
    </div>
    <div class="mb-7 flex justify-between">
      <button :class="{ 'opacity-50': page <= 1 }" :disabled="page <= 1" @click="changePage(page - 1)"
        class="border border-gray-300 rounded px-2 py-0.5 hover:bg-gray-200">Previous</button>
      <button :class="{ 'opacity-50': page >= totalItems / ITEMS_PER_PAGE }"
        :disabled="page >= totalItems / ITEMS_PER_PAGE" @click="changePage(page + 1)"
        class="border border-gray-300 rounded px-2 py-0.5 hover:bg-gray-200">Next</button>
    </div>
    <CountryList v-if="filterCountry.length > 0 || search === ''" :countries="paginatedCountries" />
    <p class="text-center text-6xl font-bold text-red-500" v-else>No results found.</p>
  </div>
</template>
