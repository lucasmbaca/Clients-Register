<script setup>
import {RouterLink} from 'vue-router'

defineProps({
    to:{
        type:String
    }
})
</script>

<template>
    <RouterLink 
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded shadow"
        :to="{name:to}"
    >
      
        <slot></slot>
    </RouterLink>

    
</template>

