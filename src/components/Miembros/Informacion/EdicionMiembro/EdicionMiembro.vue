<template>
    <div class="contenedor_edicion_miembro">
        <div class="top_edicion_miembro">
            <b-button type="button" variant="outline-danger" @click="cerrar" size="sm">Cerrar</b-button>
        </div>
        <div class="cuerpo_edicion">
            <div class="aside_cuerpo_edicion">
                <div class="btn_edicion" @click="setSeccionFunc('datosPersonales')">
                    Datos personales
                </div>
                <div class="btn_edicion" @click="setSeccionFunc('datosIglesia')">
                    Datos Iglesia
                </div>
                <div class="btn_edicion" @click="setSeccionFunc('datosHijos')">
                    Datos Hijos
                </div>
            </div>
            <div class="formulario_edicion">

                <DatosPersonales v-if="setSeccion == 'datosPersonales'" />
                <DatosIglesia v-if="setSeccion == 'datosIglesia'" />
                <DatosHijos v-if="setSeccion == 'datosHijos'" />

            </div>
        </div>
    </div>
</template>

<script>

import DatosPersonales from './EdicionDatosPersonales.vue'
import DatosIglesia from './EdicionDatosIglesia.vue'
import DatosHijos from './EdicionDatosHijos.vue'

export default {
    name: 'EdicionMiembro',
    components: {
        DatosPersonales,
        DatosIglesia,
        DatosHijos
    },
    data() {
        return {
            setSeccion: ''
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        },
        setSeccionFunc(sec){
            this.setSeccion = sec
        }
    },
}
</script>

<style>
    .contenedor_edicion_miembro{
        width: 100%;
        height: 100vh;
        position: fixed;
        top: 0;
        left: 0;
        background-color: white;
        z-index: 999;
    }
        .top_edicion_miembro{
            width: 100%;
            height: 40px;
            display: flex;
            align-items: center;
            flex-direction: row-reverse;
            padding-right: 10px;
            border-bottom: 1px solid #e6e6e6;
        }
            .cuerpo_edicion{
                width: 100%;
                height: calc(100% - 40px);
                display: flex;
            }
                .aside_cuerpo_edicion{
                    width: 150px;
                    height: 200%;
                    border-right: 1px solid #e6e6e6;
                }
                    .btn_edicion{
                        width: 100%;
                        height: 45px;
                        border-bottom: 1px solid #e6e6e6;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        cursor: pointer;
                        user-select: none;
                        transition: .4s;
                    }
                        .btn_edicion:hover{
                            background-color: purple;
                            color: white;
                        }
                .formulario_edicion{
                    width: calc(100% - 150px);
                    height: 100%;
                    overflow: auto;
                }
</style>