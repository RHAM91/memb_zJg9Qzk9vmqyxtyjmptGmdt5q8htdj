<template>

    <!-- MODULOS A ESCOGER -->

    <div class="contenedor_inicio">
        <div class="top_verde_">
            
        </div>
        <div class="cuerpo_inicio_modulos">
            <div class="modulos">

                <div class="modulo" @click="set_ruta('Miembros')">
                    <div class="titulo_modulo">
                        MIEMBROS
                    </div>
                    <div class="cuerpo_modulo">
                        <i class="fas fa-users"></i>
                    </div>
                </div>

                <div class="modulo" @click="set_ruta('Logs')">
                    <div class="titulo_modulo">
                        LOGS
                    </div>
                    <div class="cuerpo_modulo">
                        <i class="fas fa-clipboard-list"></i>
                    </div>
                </div>

            </div>

            <div class="cerrar_sesion_btn" @click="salir">
                <i class="fas fa-sign-out-alt"></i>
            </div>

        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { pregunta } from '../components/functions/alertas'
import { IP, PUERTO } from '@/config/parametros'

export default {
    name: 'Principal',
    data() {
        return {
            
        }
    },
    methods: {
        set_ruta(ruta){
            this.$router.replace(ruta)
        },
        salir(){
            pregunta({titulo: 'Seguro que deseas salir?', texto: 'Está a punto de salir del sistema', afirmacion: 'Si, salir!'}, async (i) =>{

                if (i) {
                    localStorage.removeItem('kat')
                    this.$router.replace('Login')

                    let f = {
                        datos: null
                    }

                    let b = await axios.post(`http://${IP}:${PUERTO}/api/login/cerrarsesion`, f, this.$store.state.token)
                    console.log(b.data)
                }
            })
        },
    },
}
</script>

<style>
    .contenedor_inicio{
        width: 100%;
        height: 100vh;
    }

    .top_verde_{
        background-color: #49C466;
        width: 100%;
        height: 25px;
        border-bottom: 1px solid #2ab74a;
    }

    .cuerpo_inicio_modulos{
        width: 100%;
        height: calc(100% - 25px);
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .modulos{
        width: 500px;
        height: 500px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
        .modulo{
            width: 200px;
            height: 200px;
            margin-right: 10px;
            border: 1px solid #e9e9e9;
        }
            .titulo_modulo{
                width: 100%;
                height: 25px;
                background-color: #252935;
                color: white;
                text-align: center;
            }
            
            .cuerpo_modulo{
                width: 100%;
                height: calc(100% - 25px);
                display: flex;
                justify-content: center;
                align-items: center;
                font-size: 70px;
                cursor: pointer;
                transition: .4s ease;
            }
                .cuerpo_modulo:hover{
                    background-color: #252935;
                    color: white;
                }

        .cerrar_sesion_btn{
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background-color: #e9e9e9;
            position: fixed;
            bottom: 15px;
            left: 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
            transition: .4s ease;
        }
            .cerrar_sesion_btn:hover{
                background-color: #252935;
                color: white;
            }
</style>