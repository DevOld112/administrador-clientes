<script setup>
import { onMounted } from 'vue';
import { reactive } from 'vue';
import ClienteService from '../services/ClienteServices'
import { useRouter, useRoute } from 'vue-router'
import { FormKit } from '@formkit/vue'
import RouterLink from '../components/UI/RouterLink.vue';
import Heading from '../components/UI/Heading.vue';


const router = useRouter();
const route = useRoute();

const { id } = route.params

const formData = reactive({})


onMounted(() =>{
    ClienteService.obtenerCliente(id)
    .then(({data}) => {
        formData.nombre = data.nombre
        formData.apellido = data.apellido
        formData.email = data.email
        formData.telefono = data.telefono
        formData.empresa = data.empresa
        formData.puesto = data.puesto
    })
    .catch(error => console.log(error))
})


defineProps({
    titulo:{
        type: String
    }
})

const handleSubmit = (data) =>{
    ClienteService.actualizarCliente(id, data)
    .then(() => router.push({name: 'listado-clientes'}))
    .error(error => console.log(error))
}

</script>



<template>
    <div>
        <div class="flex justify-end">
            <RouterLink 
            to="listado-clientes">
                Volver
            </RouterLink>
        </div>

        <Heading>{{ titulo }}</Heading>

        <div class="mx-auto mt-10 bg-gray-200 shadow mb-10">
            <div class="mx-auto md:w-2/3 py-20 px-6">
            <FormKit
            type="form"
            submit-label="Guardar Cambios"
            incomplete-message="Algun Campo Obligatorio esta vacio, por favor revisa."
            @submit="handleSubmit"
            :value="formData"
            >

            <FormKit 
            type="text"
            label="Nombre"
            name="nombre"
            placeholder="Nombre del cliente"
            validation="required"
            :validation-messages="{ required: 'El Nombre del Cliente es Obligatorio'}"
            v-model="formData.nombre"
            />

            <FormKit 
            type="text"
            label="Apellido"
            name="apellido"
            placeholder="Apellido del cliente"
            validation="required"
            :validation-messages="{ required: 'El Apellido del Cliente es Obligatorio'}"
            v-model="formData.apellido"
            />

            <FormKit 
            type="email"
            label="Email"
            name="email"
            placeholder="Email del cliente"
            validation="required|email"
            :validation-messages="{ required: 'El Email del Cliente es Obligatorio', email: 'Coloca un Email Valido'}"
            v-model="formData.email"
            />

            <FormKit 
            type="text"
            label="Telefono"
            name="telefono"
            placeholder="Telefono: XXX-XXX-XXXX"
            validation="required|*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
            :validation-messages="{ matches: 'El formato de telefono no es valido'}"
            v-model="formData.telefono"
            />

            <FormKit 
            type="text"
            label="Empresa"
            name="empresa"
            placeholder="Empresa del cliente"
            v-model="formData.empresa"
            />

            <FormKit 
            type="text"
            label="Puesto"
            name="puesto"
            placeholder="Puesto del cliente"
            v-model="formData.puesto"

            />



            </FormKit>
        </div>
        </div>
    </div>
</template>

<style>
    .formkit-wrapper {
        max-width: 100%;
    }
</style>