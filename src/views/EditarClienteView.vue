<script setup>
    import RouterLink from '../components/UI/RouterLink.vue';
    import { FormKit } from '@formkit/vue';
    import ClientesService from '../services/ClientesService';

    import { useRouter, useRoute } from 'vue-router';
    import { onMounted, reactive } from 'vue';

    defineProps({
        titulo:{
            type:String
        },
    })

    const formData = reactive({})

    const router = useRouter()
    const route = useRoute()

    console.log(route.params)
    const { id } = route.params

    onMounted(() => {
        ClientesService.obtenerCliente(id)
            .then(({data}) => Object.assign(formData,data))
            .catch(error => console.log)
    })

    const handlerInfo = data => {
        ClientesService.actualizarCliente(id, data)
            .then(() => router.push({name:'inicio'}))
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
                submit-label="Editar Cliente"
                @submit="handlerInfo"
                :value="formData"
                >
                <FormKit
                    type="text"
                    placeholder="Nombre del cliente"
                    label="Nombre Cliente"
                    name="nombre"
                    validation="required"
                    :validation-messages="{required:'El nombre es necesario para registrar'}"
                    v-model="formData.nombre"
                />

                <FormKit
                    type="text"
                    placeholder="Apellido del cliente"
                    label="Apellido Cliente"
                    name="apellido"
                    validation="required"
                    :validation-messages="{required:'El apellido es necesario para registrar'}"
                    v-model="formData.apellido"
                />

                <FormKit
                    type="text"
                    placeholder="Nombre del cliente"
                    label="Email Cliente"
                    name="email"
                    validation="required|email"
                    :validation-messages="{required:'el nombre es necesario para registrar',email:'Ingresa un email valido'}"
                    v-model="formData.email"
                />

                <FormKit
                    type="text"
                    placeholder="XXX-XXX-XXXX"
                    label="Telefono"
                    name="telefono"
                    validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
                    :validation-messages="{matches:'formato no valido'}"
                    v-model="formData.telefono"
                />

                <FormKit
                    type="text"
                    placeholder="Empresa del cliente"
                    name="empresa"
                    label="Empresa"
                    v-model="formData.empresa"
                />

                <FormKit
                    type="text"
                    placeholder="Puesto del cliente"
                    name="cargo"
                    label="Cargo"
                    v-model="formData.cargo"
                />
                </FormKit>
            </div>
        </div>

    </div>
</template>
