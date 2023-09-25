<script setup>
import RouterLink from '../components/UI/RouterLink.vue';
import { FormKit } from '@formkit/vue';
import ClientesService from '../services/ClientesService';

import { useRouter } from 'vue-router';

defineProps({
    titulo:{
        type:String
    },
})

const router = useRouter()

const handlerInfo = data => {
    data.estado = 1
    ClientesService.crearCliente(data)
        .then(response => {
            console.log(response)
            //REDIRECT
            router.push({name:'inicio'})
        })
        .catch(error => console.log(error))
}

</script>

<template>
    <div>
        <div class="float-right">
            <RouterLink to="inicio">
               Volver
            </RouterLink>
        </div>
        <h1 class="text-4xl font-extrabold text-slate-500">{{titulo}}</h1>

        <div class="mx-auto mt-10 bg-white">
            <div class="mx-auto md:w-1/2 py-20 px-6">
                <FormKit
                type="form"
                submit-label="Agregar Cliente"
                @submit="handlerInfo"
                >
                <FormKit
                    type="text"
                    placeholder="Nombre del cliente"
                    label="Nombre Cliente"
                    name="nombre"
                    validation="required"
                    :validation-messages="{required:'El nombre es necesario para registrar'}"
                />

                <FormKit
                    type="text"
                    placeholder="Apellido del cliente"
                    label="Apellido Cliente"
                    name="apellido"
                    validation="required"
                    :validation-messages="{required:'El apellido es necesario para registrar'}"
                />

                <FormKit
                    type="text"
                    placeholder="Nombre del cliente"
                    label="Email Cliente"
                    name="email"
                    validation="required|email"
                    :validation-messages="{required:'el nombre es necesario para registrar',email:'Ingresa un email valido'}"
                />

                <FormKit
                    type="text"
                    placeholder="XXX-XXX-XXXX"
                    label="Telefono"
                    name="telefono"
                    validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
                    :validation-messages="{matches:'formato no valido'}"
                />

                <FormKit
                    type="text"
                    placeholder="Empresa del cliente"
                    name="empresa"
                    label="Empresa"
                />

                <FormKit
                    type="text"
                    placeholder="Puesto del cliente"
                    name="cargo"
                    label="Cargo"
                />
                </FormKit>
            </div>
        </div>

    </div>
</template>

