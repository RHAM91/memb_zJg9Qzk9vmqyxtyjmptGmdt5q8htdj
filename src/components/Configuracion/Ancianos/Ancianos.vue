<template>
    <b-container fluid="">
        <form @submit.prevent="guardar">
            <b-row>
                <b-col sm="12" class="mt-3">
                    <h3>
                        Ancianos
                    </h3>
                </b-col>
                <b-col v-if=" tipo == 'root' || permisos.ancianos.crear == 1" sm="2" class="mt-4">
                    <b-button type="button" variant="info"  block size="sm" @click="abrir_modal_buscar_anciano">Buscar</b-button>
                </b-col>
                <b-col v-if=" tipo == 'root' || permisos.ancianos.crear == 1" sm="8" class="mt-4">
                    <b-form-input type="text" size="sm" v-model="nombreanciano" readonly placeholder="Nombre del anciano" id="nombreanciano" autocomplete="off" required></b-form-input>
                </b-col>
                <b-col v-if=" tipo == 'root' || permisos.ancianos.crear == 1" sm="2" class="mt-4">
                    <b-button type="submit" block size="sm"  variant="success">Guardar</b-button>
                </b-col>
                <div class="marco_">
                    <b-col sm="12" class="mt-4">
                        <table class="table table-sm table-striped table-bordered" style="font-size: 12px;">
                            <thead>
                                <tr>
                                    <th style="width: 75%;">
                                        Nombre del anciano
                                    </th>
                                    <th style="width: 5%;text-align: center;">
                                        ID
                                    </th>
                                    <th style="text-align: center;width: 20%;">
                                        ...
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in ancianos" :key="index">
                                    <td>
                                        {{item.anciano}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.id}}
                                    </td>
                                    <td style="text-align: center;">
                                        <b-button v-if=" tipo == 'root' || permisos.ancianos.actualizar == 1" type="button" size="sm" style="margin-right: 10px;" variant="primary" @click="AbrirModalActualizar(item)"><i class="fas fa-pencil-alt"></i></b-button>
                                        <b-button v-if=" tipo == 'root' || permisos.ancianos.borrar == 1" type="button" size="sm" variant="danger" @click="Eliminar(item.id)"><i class="fas fa-trash-alt"></i></b-button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </b-col>
                </div>
            </b-row>
        </form>

        <ModalActualizar v-if="actualizar" :itemAnciano="itemAnciano" v-on:salir="cerrarModalActualizar" />
        <BuscarAnciano v-if="modal_buscar_anciano" v-on:salir="cerrar_modal_buscar_anciano" v-on:seleccion="setearInformacion" />

    </b-container>
</template>

<script>

import { mapActions, mapState } from 'vuex'
import { pregunta } from '@/components/functions/alertas'

import ModalActualizar from './Actualizar.vue'
import BuscarAnciano from '../../Asignacion/Seleccion/Buscar.vue'

export default {
    name: 'Ancianos',
    computed: {
        ...mapState(['ancianos', 'permisos', 'tipo'])
    },
    components:{
        ModalActualizar,
        BuscarAnciano
    },
    data() {
        return {
            actualizar: false,
            itemAnciano: '',
            nombreanciano: '',
            dpi: '',
            modal_buscar_anciano: false
        }
    },
    methods: {
        async guardar(){
            let data = {
                api: 'ancianos',
                pull: true,
                formulario: {
                    anciano: this.nombreanciano.toUpperCase(),
                    dpi: this.dpi
                }
            }

            await this.saveData(data)
            this.nombreanciano = ''
            document.getElementById('nombreanciano').focus()
        },
        async Eliminar(id){

            pregunta({titulo: 'Seguro que deseas borrarlo?', texto: 'Esta acción no se puede revertir', afirmacion: 'Si, borrarlo!'}, async (i) =>{

                if (i) {
                    let f = {
                        api: 'ancianos',
                        id,
                        pull: true
                    }

                    await this.deleteData(f)
                }
            })
            
        },
        AbrirModalActualizar(itp){
            this.itemAnciano = itp
            this.actualizar = true

        },
        cerrarModalActualizar(){
            this.actualizar = false
        },
        abrir_modal_buscar_anciano(){
            this.modal_buscar_anciano = true
        },
        cerrar_modal_buscar_anciano(){
            this.modal_buscar_anciano = false
        },
        setearInformacion(m){
            this.nombreanciano = m.nombre
            this.dpi = m.dpi
        },
        ...mapActions(['deleteData', 'saveData', 'getData'])
    },
    mounted() {
        document.getElementById('nombreanciano').focus()
    },
}
</script>

<style>

</style>