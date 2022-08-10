<template>
    <b-container fluid>
        <b-row>
            <b-col sm="12" class="mt-3">
                <h3>
                    Pendientes de aprobación
                </h3>
            </b-col>
            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                    <thead>
                        <tr>
                            <th style="width: 15%;">
                                DPI
                            </th>
                            <th style="width: 55%;">
                                Nombre
                            </th>
                            <th style="width: 15%;text-align: center;">
                                Fecha
                            </th>
                            <th style="width: 15%;text-align: center;">
                                ...
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in datos_aprobar_miembros" :key="index">
                            <td>
                                {{item.dpi}}
                            </td>
                            <td>
                                {{item.nombre}}
                            </td>
                            <td style="text-align: center;">
                                {{item.fecha_asignacion | nFecha}}
                            </td>
                            <td style="text-align: center;">
                                <b-button type="button" variant="primary" size="sm" @click="detalle(item)"><i class="fas fa-eye"></i></b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>

        <ModalDetalleEntrevista v-if="modaldetalle" :ient="item" v-on:salir="cerrarModalDetalle" />

    </b-container>
</template>

<script>
import moment from 'moment'
import { mapState } from 'vuex'

import ModalDetalleEntrevista from './ModalDetalleEntrevista.vue'

export default {
    name: 'PendientesAprobar',
    components: {
        ModalDetalleEntrevista
    },
    computed: {
        ...mapState(['datos_aprobar_miembros'])
    },
    filters:{
        nFecha: function(val){
            return moment(val).format('DD/MM/YYYY')
        }
    },
    data() {
        return {
            modaldetalle: false,
            item: {}
        }
    },
    methods: {
        detalle(i){
            this.modaldetalle = true
            this.item = i
        },
        cerrarModalDetalle(){
            this.modaldetalle = false
        }
    },
}
</script>

<style>

</style>