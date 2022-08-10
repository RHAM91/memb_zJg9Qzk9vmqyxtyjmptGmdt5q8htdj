<template>
    <b-container fluid="">
        <b-row>
            <b-col sm="12" class="mt-3">
                <h2>
                    Borrar Miembros
                </h2>
            </b-col>
            <!-- <b-col sm="10" class="mt-3">
                <label for="">DPI</label>
                <b-form-input type="text" v-model="dpi" id="campo_buscar_dpi_para_borrar" @keyup.113="abrirModalBuscarMiembro" placeholder="F2 (para buscar)" size="sm"></b-form-input>
            </b-col> -->
            <b-col sm="2" class="mt-5">
                <b-button type="button" variant="info" @click="abrirModalBuscarMiembro" block size="sm">Agregar</b-button>
            </b-col>

            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-striped table-bordered" style="font-size: 11px">
                    <thead>
                        <tr>
                            <th style="width: 20%;">
                                DPI
                            </th>
                            <th style="width: 70%;">
                                Nombre
                            </th>
                            <th style="width: 10%;text-align:center;">
                                ...
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in bolsa" :key="index">
                            <td>
                                {{item.dpi}}
                            </td>
                            <td>
                                {{item.nombre}}
                            </td>
                            <td style="text-align: center;">
                                <b-button type="button" variant="danger" size="sm" @click="eliminar_seleccion(index)"><i class="fas fa-trash-alt"></i></b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
            <b-col v-if="bolsa.length != 0" sm="12" class="mt-3 d-flex flex-row-reverse">
                <b-button type="button" variant="danger" size="sm" @click="enviar_lista">Eliminar selección</b-button>
            </b-col>
        </b-row>

        <BuscarMiembro v-if="modalBuscarMiembro" v-on:salir="cerrarModalBuscarMiembro" v-on:seleccion="insertarDato" />

    </b-container>
</template>

<script>
import { mapActions } from 'vuex'
import { minix, pregunta } from '../../functions/alertas'

import BuscarMiembro from './BuscarMiembro.vue'


export default {
    name: "BorrarMiembros",
    components:{
        BuscarMiembro
    },
    data() {
        return {
            bolsa: [],
            modalBuscarMiembro: false,
        }
    },
    methods: {
         cerrarModalBuscarMiembro(){
            this.modalBuscarMiembro = false
        },
        abrirModalBuscarMiembro(){
            this.modalBuscarMiembro = true
        },
        insertarDato(ix){

            let evalu = false

            for (let i = 0; i < this.bolsa.length; i++) {
                const e = this.bolsa[i];
                
                if (e.dpi == ix.dpi) {
                    evalu = true
                }else{
                    evalu = false
                }

            }

            if (evalu == true) {
                minix({icon: 'error', mensaje: 'Ya se agregó este registro', tiempo: 3000})
            }else{
                let f = {
                        dpi: ix.dpi,
                        nombre: ix.nombre
                    }
        
                    this.bolsa.push(f)
            }
            
            // document.getElementById('campo_buscar_dpi_para_borrar').focus()
        },
        eliminar_seleccion(index){
            this.bolsa.splice(index, 1)
        },
        async enviar_lista(){

            pregunta({titulo: 'Seguro que borrarlo?', texto: 'Está acción no puede revertirse', afirmacion: 'Si, borrarlo!'}, async (i) =>{

                if (i) {
                    let f = {
                        api: 'miembros/d/borrar_por_lista',
                        pull: false,
                        formulario: {
                            bolsa: this.bolsa
                        }
                    }

                    await this.saveData(f)
                    this.bolsa = []
                }
            })

            
        },
        ...mapActions(['saveData'])
    },
}
</script>

<style>

</style>