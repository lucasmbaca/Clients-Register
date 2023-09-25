<script setup>
import { computed } from 'vue';
import { RouterLink } from 'vue-router';

const props = defineProps({
    cliente:{
        type: Object
    }
})

defineEmits(['actualizar-estado', 'eliminar-cliente'])

const nombreCompletoCliente = computed(() => {
    return `${props.cliente.nombre} ${props.cliente.apellido}`
}) 

const controlEstado = computed(() => {
    return props.cliente.estado
})


</script>

<template>
    <tr>
        <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm sm:pl-0">
            <p class="font-medium text-gray-900">{{ nombreCompletoCliente }}</p>
            <p class="text-gray-500">{{ cliente.email }}</p>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
            <p class="text-gray-900 font-bold">{{ cliente.empresa }}</p>
            <p class="text-gray-600">{{ cliente.cargo }}</p>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm">
            <button
                @click="$emit('actualizar-estado', {id:cliente.id, estado:cliente.estado})"
               class="text-xs rounded-full py-2 px-4 text-white"
               :class="[controlEstado? 'bg-green-500 hover:bg-green-600':'bg-red-500 hover:bg-red-600']"
            >
                {{ controlEstado ? 'activo':'inactivo' }}
            </button>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 ">
         <RouterLink
            :to="{name:'editar-cliente', params:{id:cliente.id}}"
            class="text-indigo-500 hover:text-indigo-900 mr-5"
         >
         editar</RouterLink>
         <button
            class="text-red-500 hover:text-red-800"
            @click="$emit('eliminar-cliente', cliente.id)"
         >
            Eliminar
         </button>
        </td>
    </tr>
</template>