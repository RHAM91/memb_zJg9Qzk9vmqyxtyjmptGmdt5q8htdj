<template>
    <b-container fluid>
        <form @submit.prevent="guardarAsignaciones">
            <b-row>
                <b-col sm="12" class="mt-3">
                    <h2>
                        Asignación de áreas y privilegios
                    </h2>
                </b-col>
                <b-col sm="2" class="mt-3">
                    <b-button type="button" id="campo_buscar_miembro" size="sm" block variant="info" @click="abrirModalBuscarMiembro">Buscar</b-button>
                </b-col>
                <b-col sm="10" class="mt-3">
                    <b-form-input type="text" v-model="nombre" readonly size="sm" required></b-form-input>
                </b-col>
                <b-col sm="5" class="mt-3">
                    <label for="">Área</label>
                    <select id="asig_area" class="form-control form-control-sm" v-model="area">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in areas" :key="index" :value="item.id">{{item.area}}</option>
                    </select>
                </b-col>
                <b-col sm="5" class="mt-3">
                    <label for="">Privilegio</label>
                    <select id="asig_priv" class="form-control form-control-sm" v-model="priv">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in privilegios" :key="index" :value="item.id">{{item.privilegio}}</option>
                    </select>
                </b-col>
                <b-col sm="2" class="mt-5">
                    <b-button type="button" variant="outline-success" block size="sm" @click="validarExistenciaPrivilegio">Agregar</b-button>
                </b-col>

                <b-col sm="12" class="mt-4">
                    <table class="table table-sm table-striped table-bordered" style="font-size: 11px">
                        <thead>
                            <tr>
                                <th style="width: 45%;">
                                    Área
                                </th>
                                <th style="width: 45%;">
                                    Privilegio
                                </th>
                                <th style="width: 10%;text-align: center;">
                                    ...
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(item, index) in bolsa" :key="index">
                                <td>
                                    {{item.dns_area[0].area}}
                                </td>
                                <td>
                                    {{item.dns_priv[0].privilegio}}
                                </td>
                                <td style="text-align: center;">
                                    <b-button type="button" size="sm" variant="danger" @click="borrarFila(index)"><i class="fas fa-trash-alt"></i></b-button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </b-col>

                <b-col v-if="bolsa.length != 0" sm="12" class="mt-3 d-flex flex-row-reverse">
                    <b-button type="submit" size="sm" variant="success">Asignar</b-button>
                </b-col>
            </b-row>
        </form>

        <BuscarMiembro v-if="modalBuscar" v-on:salir="salirModalBuscarMiembro" v-on:seleccion="setearInformacion" />

    </b-container>
</template>

<script>
import { mapActions, mapState } from 'vuex'
import { minix } from '../../functions/alertas'

import BuscarMiembro from '../Seleccion/Buscar.vue'

export default {
    name: 'SeleccionarMiembro',
    computed: {
        ...mapState(['areas', 'privilegios', 'permisos', 'tipo'])
    },
    filters: {
        f_areas: function(val){
            let r = this.convertir({array: 'areas', valor: 'id', parametro: val})
            return r
        }
    },
    components: {
        BuscarMiembro
    },
    data() {
        return {
            modalBuscar: false,
            nombre: '',
            dpi: '',
            area: '',
            priv: '',
            bolsa: []
        }
    },
    methods: {
        abrirModalBuscarMiembro(){
            this.modalBuscar = true
        },
        salirModalBuscarMiembro(){
            this.modalBuscar = false
            document.getElementById('asig_area').focus()
        },
        setearInformacion(i){
            this.nombre = i.nombre
            this.dpi = i.dpi
        },
        async agregarAsignacion(){
            if (this.nombre == '') {
                minix({icon: 'error', mensaje: 'Sebes seleccionar un miembro', tiempo: 3000})
            }else{
                if (this.area == '') {
                    minix({icon: 'error', mensaje: 'Área es obligatorio', tiempo: 3000})
                    document.getElementById('asig_area').focus()
                }else if (this.priv == '') {
                    minix({icon: 'error', mensaje: 'Privilegio es obligatorio', tiempo: 3000})
                    document.getElementById('asig_priv').focus()
                }else{
                    let h = {
                        area: this.area,
                        dns_area: await this.convertir({array: 'areas', valor: 'id', parametro: this.area}),
                        privilegio: this.priv,
                        dns_priv: await this.convertir({array: 'privilegios', valor: 'id', parametro: this.priv})
                    }

                    this.bolsa.push(h)

                    this.area = ''
                    this.priv = ''

                    document.getElementById('asig_area').focus()
                }
            }
        },
        async guardarAsignaciones(){
            let f = {
                api: 'asignaciones',
                pull: false,
                formulario: {
                    dpi: this.dpi,
                    bolsa: this.bolsa
                }
            }

            await this.saveData(f)
            this.dpi = ''
            this.nombre = ''
            this.area = ''
            this.priv = ''
            this.bolsa = []
        },
        async validarExistenciaPrivilegio(){ // PENDIENTE VALIDAR SI SE INGRESA DOBLE EN LA LISTA
            let r = await this.getData({api: `asignaciones/validacion/?dpi=${this.dpi}&privilegio=${this.priv}&area=${this.area}`})

            if (r.message == 'PUEDE ASIGNARSE') {
                this.agregarAsignacion()
            }
        },
        borrarFila(i){
            this.bolsa.splice(i, 1)
        },
        ...mapActions(['convertir', 'saveData', 'getData'])
    },
    mounted() {
        document.getElementById('campo_buscar_miembro').focus()
    },
}
</script>

<style>

</style>