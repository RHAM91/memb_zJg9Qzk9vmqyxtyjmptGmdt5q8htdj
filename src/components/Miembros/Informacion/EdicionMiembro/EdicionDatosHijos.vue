<template>
    <b-container fluid="">
        <b-row>
            <b-col sm="12" class="mt-3">
                <h3>
                    Edición datos hijos
                </h3>
            </b-col>
            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                    <thead>
                        <tr>
                            <th style="width: 75%">
                                Nombre
                            </th>
                            <th style="width: 15%;text-align: center;">
                                Fecha de nacimiento
                            </th>
                            <th style="width: 10%;text-align: center;">
                                ...
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in bolsa" :key="index">
                            <td>
                                {{item.nombre}}
                            </td>
                            <td style="text-align: center;">
                                {{item.fecha_de_nacimiento | nfecha}}
                            </td>
                            <td style="text-align: center;">
                                <b-button type="button" style="font-size:10px;" variant="outline-primary" size="sm" @click="abrir_modal_datos_hijos(item)"><i class="fas fa-pen"></i></b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>

        </b-row>

        <SubEdicionDatosHijos v-if="sub_modal" :item="item" v-on:salir="cerrar_modal_datos_hijos" v-on:actualizar="obtener_datos_hijos" />

    </b-container>
</template>

<script>
import moment from 'moment'
import { mapActions, mapState } from 'vuex'
import SubEdicionDatosHijos from './SubEdicionDatoHijos.vue'

export default {
    name: 'EdicionDatosHijos',
    computed: {
        ...mapState(['datos_edicion_miembro'])
    },
    components:{
        SubEdicionDatosHijos
    },
    filters:{
        nfecha: function(val){
            return moment(val).format('DD/MM/YYYY')
        }
    },
    data() {
        return {
            bolsa: [],
            sub_modal: false,
            item: {}
        }
    },
    methods: {
        async obtener_datos_hijos(){
            let f ={
                api: `miembros/c/datoshijos/${this.datos_edicion_miembro.dpi}`
            }

            let r = await this.getData(f)
            this.bolsa = r
        },
        cerrar_modal_datos_hijos(){
            this.sub_modal = false
        },
        abrir_modal_datos_hijos(item){
            this.sub_modal = true
            this.item = item
        },
        ...mapActions(['getData'])
    },
    mounted() {
        this.obtener_datos_hijos()
    },
}
</script>

<style>

</style>