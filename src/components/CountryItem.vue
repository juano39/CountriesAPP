<script setup lang="ts">

import { defineProps } from 'vue'
import { Country } from '../models/country.model';

interface Props {
    country: Country
}

defineProps<Props>()

</script>


<template>
    <div class="h-56 mb-3 ">
        <img class="h-52 mx-auto" :src="country.flags.png" alt="country.flags.alt">
    </div>
    <h4 class="text-xl font-bold text-gray-500">{{ country.name.common }}</h4>
    <p class="text-sm text-gray-400">Capital: <span v-for="(capital, index) in country.capital" :key="index"
            class="font-bold">{{ capital
            }} <template></template></span> </p>
    <p class="text-sm text-gray-400">Population: <span class="font-bold">{{ country.population?.toLocaleString()
            }}</span></p>
    <p class="text-sm text-gray-400">Region: <span class="font-bold">{{ country.region }}</span></p>
</template>