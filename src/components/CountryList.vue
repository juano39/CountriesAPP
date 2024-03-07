<script setup lang="ts">

import { defineProps } from 'vue';
import { Country } from '../models/country.model';
import Countryitem from './CountryItem.vue';

interface Props {
    countries: Country[]
}

defineProps<Props>()

</script>



<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-3">
        <div v-for="country in countries" :key="country.name.common" class="border border-gray-300 rounded p-3">
            
            <Countryitem :country="country"/>
        </div>
    </div>
</template>