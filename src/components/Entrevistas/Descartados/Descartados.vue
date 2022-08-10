<template>
    <b-container fluid="">
        <b-row>
            <b-col sm="12" class="mt-3">
                <h3>
                    Entrevistas descartadas
                </h3>
            </b-col>
            <b-col sm="12" class="mt-3">
                <table class="table table-striped table-sm table-bordered" style="font-size: 11px;">
                    <thead>
                        <tr>
                            <th style="width: 15%;">
                                DPI
                            </th>
                            <th style="width: 55%">
                                Nombre
                            </th>
                            <th style="width: 15%; text-align: center;">
                                Fecha creación
                            </th>
                            <th style="width: 15%;text-align: center;">
                                Reasignar
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in descartes" :key="index">
                            <td>
                                {{item.dpi}}
                            </td>
                            <td>
                                {{item.nombre}}
                            </td>
                            <td style="text-align: center;">
                                {{item.fecha | nfecha}}
                            </td>
                            <td style="text-align: center;">
                                <b-button type="button" title="Reasignar" variant="warning" size="sm" @click="abrir_asignacion(item)"> <i class="fas fa-redo"></i> </b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>

        <ModalAsignar v-if="modal_asignar" :item="dpi" v-on:salir="cerrar_asignacion" />

    </b-container>
</template>

<script>
import moment from 'moment'
import { mapActions } from 'vuex'

import ModalAsignar from '../Asignacion/ModalParaAsginar.vue'

export default {
    name: 'Descartados',
    components:{
        ModalAsignar
    },
    filters:{
        nfecha: function(val){
            return moment(val).format('DD/MM/YYYY')
        }
    },
    data() {
        return {
            descartes: [],
            modal_asignar: false,
            dpi: ''
        }
    },
    methods: {
        async obtenerEntrevistasDescartadas(){
            let r = await this.getData({api: 'asignacion_entrevistas/c/descartados'})
            this.descartes = r
        },
        cerrar_asignacion(){
            this.modal_asignar = false
        },
        abrir_asignacion(dp){
            this.dpi = dp
            this.modal_asignar = true
        },
        ...mapActions(['getData'])
    },
    mounted() {
        this.obtenerEntrevistasDescartadas()
    },
}
</script>

<style>

</style>