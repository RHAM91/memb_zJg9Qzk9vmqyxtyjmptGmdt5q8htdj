<template>
    <div class="contenedor_modal">
        <div class="cuerpo_modal">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" size="sm" @click="cerrar">Cerrar</b-button>
            </div>

            <RegDatosIglesia />

        </div>
    </div>
</template>

<script>

import RegDatosIglesia from '../Registro/DatosIglesia.vue'

export default {
    name: 'ModalRegistroDatosIglesia',
    components:{
        RegDatosIglesia
    },
    data() {
        return {
            
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        }
    },
}
</script>

<style>

</style>