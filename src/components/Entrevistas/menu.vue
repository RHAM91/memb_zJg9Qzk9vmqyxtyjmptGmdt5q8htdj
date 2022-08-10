<template>
    
    <div class="seccion_b">
        <div class="sub_menu_a">


            <div class="top_cuerpo_seccion_b">
                Entrevistas pendientes
            </div>

            <div class="submenu_titulo">
                Actividades
            </div>

            <div class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fas fa-clipboard-list"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('entrevistas_pendientes')">
                    Pendientes de asignar
                </div>
            </div>

            <div class="submenu_titulo">
                Consultas
            </div>

            <div class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fas fa-ban"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('descartados')">
                    Descartes
                </div>
            </div>

        </div>


        <div class="cuerpo_seccion_b">
            <div class="top_cuerpo_seccion_b">
                
            </div>
            <div class="cuerpo_sub_seccion_b">
                
                <EntrevistasPendientes v-if="sub_modulo == 'entrevistas_pendientes'" />
                <Descartados v-if="sub_modulo == 'descartados'" />
            </div>
        </div>
    </div>
</template>

<script>
import { mapState } from 'vuex'

import EntrevistasPendientes from './Asignacion/EntrevistasPendientes.vue'
import Descartados from './Descartados/Descartados.vue'

export default {
    components:{
        EntrevistasPendientes,
        Descartados
    },
    computed: {
        ...mapState(['permisos', 'tipo'])
    },
    data() {
        return {
            sub_modulo: ''
        }
    },
    methods: {
        set_submodulo(modulo){
            this.sub_modulo = modulo
        }
    }
}
</script>

<style>
    
</style>