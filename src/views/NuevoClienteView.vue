<script setup>
import { reactive } from 'vue';
import ClienteService from '../services/ClienteServices'
import { useRouter} from 'vue-router'
import { FormKit } from '@formkit/vue'
import RouterLink from '../components/UI/RouterLink.vue';
import Heading from '../components/UI/Heading.vue';


const router = useRouter();



defineProps({
    titulo:{
        type: String
    }
})

const handleSubmit = (data) =>{
    data.estado = 1;
    ClienteService.agregarCliente(data)
    .then(respuesta => {
        console.log(respuesta)
        router.push({ name: 'listado-clientes' })
    })
    .catch(error => console.log(error))
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

        <div class="mx-auto mt-10 bg-gray-100 shadow mb-10">
            <div class="mx-auto md:w-2/3 py-20 px-6">
            <FormKit
            type="form"
            submit-label="Agregar Cliente"
            incomplete-message="Algun Campo Obligatorio esta vacio, por favor revisa."
            @submit="handleSubmit"
            >

            <FormKit 
            type="text"
            label="Nombre"
            name="nombre"
            placeholder="Nombre del cliente"
            validation="required"
            :validation-messages="{ required: 'El Nombre del Cliente es Obligatorio'}"

            />

            <FormKit 
            type="text"
            label="Apellido"
            name="apellido"
            placeholder="Apellido del cliente"
            validation="required"
            :validation-messages="{ required: 'El Apellido del Cliente es Obligatorio'}"

            />

            <FormKit 
            type="email"
            label="Email"
            name="email"
            placeholder="Email del cliente"
            validation="required|email"
            :validation-messages="{ required: 'El Email del Cliente es Obligatorio', email: 'Coloca un Email Valido'}"

            />

            <FormKit 
            type="text"
            label="Telefono"
            name="telefono"
            placeholder="Telefono: XXX-XXX-XXXX"
            validation="required|*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
            :validation-messages="{ matches: 'El formato de telefono no es valido'}"

            />

            <FormKit 
            type="text"
            label="Empresa"
            name="empresa"
            placeholder="Empresa del cliente"
            />

            <FormKit 
            type="text"
            label="Puesto"
            name="puesto"
            placeholder="Puesto del cliente"


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