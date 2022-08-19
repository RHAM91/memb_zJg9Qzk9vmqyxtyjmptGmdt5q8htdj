<template>
    <div class="contenedor_inicio">
        <div class="top_verde_">

        </div>
        <div class="contenedor_cuerpo_logs">
            <div class="lateral_logs">
                <div class="titulo_log">
                    Logs
                </div>
                <div class="seccion_log" @click="set_modulo('Sesiones')">
                    Sesiones
                </div>
                <div class="seccion_log" @click="mover_ruta">
                    Regresar <
                </div>
            </div>
            <div class="mostrador_logs">
                <Sesiones v-if="modulo == 'Sesiones'" />
            </div>
        </div>
    </div>
</template>

<script>

import Sesiones from '../components/Logs/Sesiones.vue'


export default {
    name: 'Logs',
    components:{
        Sesiones
    },
    data() {
        return {
            modulo: ''
        }
    },
    methods: {
        set_modulo(modulo){
            this.modulo = modulo
        },
        mover_ruta(){
            this.$router.replace('Main')
        }
    },
}
</script>

<style>
    .contenedor_cuerpo_logs{
        width: 100%;
        height: calc(100% - 25px);
        display: flex;
    }

    
        .lateral_logs{
            width: 170px;
            height: 100%;
            border-right: 1px solid #e9e9e9;
        }
        

            .seccion_log{
                width: 100%;
                height: 40px;
                border-bottom: 1px solid #e9e9e9;
                display: flex;
                align-items: center;
                justify-content: center;
                transition: .4s ease;
                cursor: pointer;
                user-select: none;
            }

                .seccion_log:hover{
                    background-color: orangered;
                    color: white;
                }

                .titulo_log{
                     width: 100%;
                    height: 40px;
                    background-color: black;
                    border: none;
                    color: white;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    transition: unset;
                    user-select: none;
                }

        .mostrador_logs{
            width: calc(100% - 170px);
            height: 100%;
        }
</style>