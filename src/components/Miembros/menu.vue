<template>
    
    <div class="seccion_b">
        <div class="sub_menu_a">


            <div class="top_cuerpo_seccion_b">
                Miembros
            </div>

            <div class="submenu_titulo">
                Registros
            </div>
            <div v-if="tipo == 'root' || permisos.miembros.ver == 1" class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fas fa-database"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('miembros')">
                    Miembros
                </div>
            </div>


            <div class="submenu_titulo">
                Consultas
            </div>
            <div v-if=" tipo == 'root' || permisos.miembros.leer == 1" class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fas fa-search"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('buscar_miembro')">
                    Buscar miembro
                </div>
            </div>

            <div class="submenu_titulo">
                Reportes
            </div>

            <div class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fas fa-file-alt"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('reportes_por_area')">
                    Reporte por área
                </div>
            </div>

            <div class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fas fa-file-alt"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('reportes_miembro')">
                    Reporte miembros
                </div>
            </div>            


        </div>


        <div class="cuerpo_seccion_b">
            <div class="top_cuerpo_seccion_b">
                
            </div>
            <div class="cuerpo_sub_seccion_b">

                <MarcaDeAgua v-if="sub_modulo == 'marca_agua'" />
                <Miembros v-if="sub_modulo == 'miembros'" /> 
                <BuscarMiembro v-if="sub_modulo == 'buscar_miembro'" />
                <ReportePorArea v-if="sub_modulo == 'reportes_por_area'" />
                <ReporteMiembro v-if="sub_modulo == 'reportes_miembro'" />
            </div>
        </div>
    </div>
</template>

<script>

//@NOTA: Se colocaron en comentario debido que el proceso ya no admitirá entrevistas

import { mapState } from 'vuex'

import Miembros from './Registro/Miembros.vue'
import BuscarMiembro from './Consultas/BuscarMiembro.vue'
import ReportePorArea from '../Reportes/ReportePorArea.vue'
import ReporteMiembro from '../Reportes/ReporteMiembros.vue'
import MarcaDeAgua from '../others/MarcaDeAgua.vue'

export default {
    components:{
       Miembros,
       BuscarMiembro,
       ReportePorArea,
       ReporteMiembro,
       MarcaDeAgua
    },
    computed: {
        ...mapState(['permisos', 'tipo'])
    },
    data() {
        return {
            sub_modulo: 'marca_agua'
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