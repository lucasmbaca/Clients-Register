<script setup>
import {ref, onMounted, computed } from 'vue';
import RouterLink from '../components/UI/RouterLink.vue';

import Clientes from '../components/Clientes.vue'
import ClientesService from '../services/ClientesService';

const clientes = ref([])

defineProps({
    titulo:{
        type:String
    },
})

onMounted(() => {
    ClientesService.obtenerClientes()
        .then(({data}) => {
            clientes.value = data
        })
        .catch(error => console.log('hubo un error'))
})

const existenClientes = computed(() => {
    return clientes.value.length > 0
})

const actualizandoEstado = ({id, estado}) => {
    ClientesService.actualizarEstadoCliente(id, {estado: !estado})
        .then(() => {
            const i = clientes.value.findIndex(cliente => cliente.id === id)
            clientes.value[i].estado = !estado
        })
        .catch(error => console.log(error))
}

const eliminarCliente = id => {
    ClientesService.eliminarCliente(id)
        .then(()=>{
            clientes.value = clientes.value.filter(cliente => cliente.id !== id)
        })
        .catch(error => console.log(error))
}

</script>

<template>
    <div>
        <div class="float-right">
            <RouterLink to="agregar-cliente">
              Agregar Cliente
            </RouterLink>
        </div>
        <h1 class="text-4xl font-extrabold text-slate-500">{{titulo}}</h1>


  <div v-if="existenClientes" class="flow-root mx-auto  mt-10 p-5 bg-white shadow">
      <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
          <div class="min-w-full py-2 align-middle sm:px-6 lg:px-8">
              <table class="min-w-full divide-y divide-gray-300">
                  <thead>
                  <tr>
                      <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Nombre</th>
                      <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Empresa</th>
                      <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Estado</th>
                      <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Acciones</th>
                  </tr>
                  </thead>
                  <tbody class="divide-y divide-gray-200 bg-white">
                    <Clientes
                        v-for="cliente in clientes"
                        @actualizar-estado="actualizandoEstado"
                        @eliminar-cliente="eliminarCliente"
                        :key="cliente.id"
                        :cliente="cliente"
                    />
                  </tbody>
              </table>
          </div>
      </div>
  </div>

  <p v-else class="text-center mt-10">
    No existen clientes
  </p>

    </div>
</template>


