<template>
    
    <div class="seccion_b">
        <div class="sub_menu_a">


            <div class="top_cuerpo_seccion_b">
                Asignación de privilegios
            </div>

            <div class="submenu_titulo">
                Acciones
            </div>
            <div v-if="tipo == 'root' || permisos.miembros.ver == 1" class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fas fa-book"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('seleccionar_miembro')">
                    Asignar privilegio
                </div>
            </div>
            <!-- <div v-if="tipo == 'root' || permisos.miembros.ver == 1" class="submenu_modulo">
                <div class="submenu_modulo_icon">
                    <i class="fa-solid fa-eye"></i>
                </div>
                <div class="submenu_modulo_texto" @click="set_submodulo('ver_asignados')">
                    Ver asignaciones
                </div>
            </div> -->

        </div>


        <div class="cuerpo_seccion_b">
            <div class="top_cuerpo_seccion_b">
                
            </div>
            <div class="cuerpo_sub_seccion_b">

                <SeleccionarMiembro v-if="sub_modulo == 'seleccionar_miembro'" />
                <VerAsignados v-if="sub_modulo == 'ver_asignados'"/>
            </div>
        </div>
    </div>
</template>

<script>
import { mapState } from 'vuex'

import SeleccionarMiembro from './Seleccion/SeleccionMiembro.vue'
import VerAsignados from './VerAsignados/verasignados.vue'

export default {
    components:{
        SeleccionarMiembro,
        VerAsignados
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