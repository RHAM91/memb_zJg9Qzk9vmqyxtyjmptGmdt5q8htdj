<template>
    <b-container fluid="">
        <form @submit.prevent="guardar">
            <b-row>
                <b-col sm="12" class="mt-3">
                    <h3>
                        Profesiones
                    </h3>
                </b-col>
                <b-col v-if=" tipo == 'root' || permisos.profesiones.crear == 1" sm="10" class="mt-4">
                    <label for="">Nombre la profesión</label>
                    <b-form-input type="text" size="sm" id="nombreprofesion" v-model="nombreprofesion" autocomplete="off" required></b-form-input>
                </b-col>
                <b-col v-if=" tipo == 'root' || permisos.profesiones.crear == 1" sm="2" class="mt-4">
                    <b-button type="submit" block size="sm" style="margin-top: 31px;" variant="success">Guardar</b-button>
                </b-col>
                <div class="marco_">
                    <b-col sm="12" class="mt-4">
                        <table class="table table-sm table-striped table-bordered" style="font-size: 12px;">
                            <thead>
                                <tr>
                                    <th style="width: 75%;">
                                        Profesión
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
                                <tr v-for="(item, index) in profesiones" :key="index">
                                    <td>
                                        {{item.profesion}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.id}}
                                    </td>
                                    <td style="text-align: center;">
                                        <b-button v-if=" tipo == 'root' || permisos.profesiones.actualizar == 1" type="button" size="sm" style="margin-right: 10px;" variant="primary" @click="AbrirModalActualizar(item)"><i class="fas fa-pencil-alt"></i></b-button>
                                        <b-button v-if=" tipo == 'root' || permisos.profesiones.borrar == 1" type="button" size="sm" variant="danger" @click="Eliminar(item.id)"><i class="fas fa-trash-alt"></i></b-button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </b-col>
                </div>
            </b-row>
        </form>

        <ModalActualizar v-if="actualizar" :itemProfesion="itemProfesion" v-on:salir="cerrarModalActualizar" />

    </b-container>
</template>

<script>

import { mapActions, mapState } from 'vuex'
import { pregunta, minix } from '@/components/functions/alertas'

import ModalActualizar from './Actualizar.vue'

export default {
    name: 'Profesiones',
    computed: {
        ...mapState(['profesiones', 'permisos', 'tipo'])
    },
    components:{
        ModalActualizar
    },
    data() {
        return {
            actualizar: false,
            itemProfesion: '',
            nombreprofesion: ''
        }
    },
    methods: {
        async guardar(){

            if (this.nombreprofesion.length > 45) {
                minix({icon: 'error', mensaje: 'El nombre es muy grande', tiempo: 3000})
                document.getElementById('nombreprofesion').focus()
                document.getElementById('nombreprofesion').select()
            }else{

                let data = {
                    api: 'profesiones',
                    pull: true,
                    formulario: {
                        profesion: this.nombreprofesion.toUpperCase()
                    }
                }
    
                await this.saveData(data)
                this.nombreprofesion = ''
                document.getElementById('nombreprofesion').focus()

            }

        },
        async Eliminar(id){

            pregunta({titulo: 'Seguro que deseas borrarlo?', texto: 'Esta acción no se puede revertir', afirmacion: 'Si, borrarlo!'}, async (i) =>{

                if (i) {
                    let f = {
                        api: 'profesiones',
                        id,
                        pull: true
                    }

                    await this.deleteData(f)
                }
            })
            
        },
        AbrirModalActualizar(itp){
            this.itemProfesion = itp
            this.actualizar = true

        },
        cerrarModalActualizar(){
            this.actualizar = false
        },
        ...mapActions(['deleteData', 'saveData'])
    }
}
</script>

<style>

</style>