<template>
    <b-container fluid="">
        <b-row>
            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                    <thead>
                        <tr>
                            <th style="width: 30%;">
                                Área
                            </th>
                            <th style="width: 40%;">
                                Privilegio
                            </th>
                            <th style="width: 20%;text-align: center;">
                                Fecha de asignación
                            </th>
                            <th style="width: 10%;text-align: center;">
                                ...
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in privs" :key="index">
                            <td>
                                {{item.area}}
                            </td>
                            <td>
                                {{item.privilegio}}
                            </td>
                            <td style="text-align: center;">
                                {{item.fecha_asignacion | nfecha}}
                            </td>
                            <td style="text-align: center;">
                                <b-button type="button" variant="danger" style="font-size: 10px;" size="sm" @click="eliminar_privilegios(item.id)"><i class="fas fa-trash"></i></b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>


        <ModalAgregarPrivilegios v-if="modal_add_privilegios" :_dpi="dpi_for_add_privs" :_nombre="nombre_for_add_privs" v-on:salir="cerrar_modal_add_privilegios" v-on:actualizar_privilegios="actualizar_privs" />

        <div class="btn_add_privilegio" @click="abir_modal_add_privilegios">
            <i class="fas fa-plus"></i>
        </div>
    </b-container>
</template>

<script>
import moment from 'moment'
import { mapActions, mapState } from 'vuex'
import ModalAgregarPrivilegios from './ModalAgregarPrivilegios.vue'


import { pregunta } from '../../functions/alertas'
export default {
    name: 'ListadoPrivilegios',
    components:{
        ModalAgregarPrivilegios
    },
    props:['iMiembro'],
    computed: {
        ...mapState(['areas', 'privilegios'])
    },
    filters:{
        nfecha: function(val){
            return moment(val).format('DD/MM/YYYY')
        }
    },
    data() {
        return {
            privs: [],
            modal_add_privilegios: false,
            dpi_for_add_privs: '',
            nombre_for_add_privs: ''
        }
    },
    methods: {
        async obtenerDatos(){
            let r = await this.getData({api: `asignaciones/${this.iMiembro.dpi}`})

            for (let i = 0; i < r.length; i++) {
                const e = r[i];
                
                let a = this.areas.filter(z => z.id == e.area)
                let p = this.privilegios.filter(h => h.id == e.privilegio)

                let nf = {
                    id: e.id,
                    area: a[0].area,
                    privilegio: p[0].privilegio,
                    fecha_asignacion: e.fecha_asignacion
                }

                this.privs.push(nf)

            }
        },
        async eliminar_privilegios(id){
            pregunta({titulo: 'Seguro que deseas eliminar el privilegio?', texto: 'Se eliminará y no podrá ser tomando en cuenta para este privilegio', afirmacion: 'Si, borrarlo!'}, async (i) =>{

                if (i) {
                    let f = {
                        api: 'asignaciones/borrar',
                        id: `?codigo=${id}&dpi=${this.iMiembro.dpi}`,
                        pull: false
                    }

                    await this.deleteData(f)
                    this.privs = []
                    await this.obtenerDatos()
                }
            })
        },
        async actualizar_privs() {
            this.privs = []
            await this.obtenerDatos()
            await this.cerrar_modal_add_privilegios()
        },
        abir_modal_add_privilegios(){
            this.modal_add_privilegios = true
        },
        cerrar_modal_add_privilegios(){
            this.modal_add_privilegios = false
        },
        ...mapActions(['getData', 'deleteData'])
    },
    mounted() {
        this.obtenerDatos()
        this.dpi_for_add_privs = this.iMiembro.dpi
        this.nombre_for_add_privs = this.iMiembro.nombre
    },
}
</script>

<style>
    .btn_add_privilegio{
        border-radius: 50%;
        width: 45px;
        height: 45px;
        position: fixed;
        bottom: 20px;
        right: 20px;
        background-color: #06ba63;
        color: white;
        display: flex;
        font-size: 22px;
        justify-content: center;
        align-items: center;
        transition: .4s ease;
        cursor: pointer;
        user-select: none;
    }
        .btn_add_privilegio:hover{
            background-color: #EF5D60;
        }
</style>