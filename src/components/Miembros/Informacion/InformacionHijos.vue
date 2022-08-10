<template>
    <b-container fluid>
        <b-row v-if="mostrar_datos_hijos">
            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-bordered table-striped" style="font-size: 11px;">
                    <thead>
                        <tr>
                            <th style="width: 55%">
                                Nombre
                            </th>
                            <th style="width: 20%;text-align: center;">
                                Fecha de nacimiento
                            </th>
                            <th style="width: 15%;text-align: center;">
                                Edad
                            </th>
                            <th style="width: 10%;text-align: center;">
                                ...
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in hijos" :key="index">
                            <td>
                                {{item.nombre}}
                            </td>
                            <td style="text-align: center;">
                                {{item.fecha_de_nacimiento | nfecha}}
                            </td>
                            <td style="text-align: center;">
                                {{item.fecha_de_nacimiento | edad}} años
                            </td>
                            <td style="text-align:center;">
                                <b-button type="button" style="font-size: 10px;" variant="danger" size="sm" @click="borrar_hijo(item.id)"><i class="fas fa-trash"></i></b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>
        <NoEncontrado v-if="mostrar_datos_hijos == false" :titulo="'No se encontraron registros'" />
        <ModalRegistroDatosHijos v-if="mostrar_registro_hijos" :dmiembro="dmiembro" v-on:actualizar="obtenerInfo" v-on:salir="cerrar_moda_registro_datos_hijos" />

        <div class="btn_agregar_formulario" @click="abrir_modal_registro_datos_hijos">
            <i class="fas fa-plus"></i>
        </div>
    </b-container>
</template>

<script>
import { mapActions } from 'vuex'
import moment from 'moment'

import NoEncontrado from '@/components/others/NoEncontrado.vue'
import { pregunta } from '../../functions/alertas'

import ModalRegistroDatosHijos from './ModalRegistroDatosHijos.vue'

export default {
    name: 'InformacionHijos',
    props: ['iMiembro'],
    components: {
        NoEncontrado,
        ModalRegistroDatosHijos
    },
    filters:{
        nfecha: function(val){
            return moment(val).format('DD/MM/YYYY')
        },
        edad: function(val){
            // const aactual = moment(Date.now()).format('YYYY')
            // return  aactual - moment(val).format('YYYY')
            const nacimiento = moment(val)
            const hoy = moment()
            return hoy.diff(nacimiento, 'years')
        }
    },
    data() {
        return {
            hijos:[],
            mostrar_datos_hijos: false,
            mostrar_registro_hijos: false,
            dmiembro: {}
        }
    },
    methods: {
        async obtenerInfo(){
            let f = await this.getData({api: `miembros/c/datoshijos/${this.iMiembro.dpi}`})

            if(f.length == 0){
                this.mostrar_datos_hijos = false
            }else{
                this.mostrar_datos_hijos = true
                this.hijos = f
            }

        },
        async borrar_hijo(id){

            pregunta({titulo: 'Seguro que deseas borrarlo?', texto: 'Está acción no se puede revertir', afirmacion: 'Si, borrarlo!'}, async (i) =>{

                if (i) {
                    
                    let f = {
                        api: 'miembros/d/hijos',
                        id,
                        pull: false
                    }
        
                    await this.deleteData(f)
                    await this.obtenerInfo()
                }
            })

        },
        abrir_modal_registro_datos_hijos(){
            this.mostrar_registro_hijos = true
        },
        cerrar_moda_registro_datos_hijos(){
            this.mostrar_registro_hijos = false
        },
        ...mapActions(['getData', 'deleteData'])
    },
    mounted() {
        this.obtenerInfo()
        this.dmiembro = this.iMiembro
    },
}
</script>

<style>

</style>