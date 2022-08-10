<template>
    <div class="contenedor_modal">
        <div class="cuerpo_modal">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" size="sm" @click="cerrar">Cerrar</b-button>
            </div>

            <b-container fluid="">
                <form @submit.prevent="guardarAsignaciones">
                    <b-row>
                        <b-col sm="12" class="mt-3">
                            <h2>
                                Asignación de áreas y privilegios
                            </h2>
                        </b-col>
                        <!-- <b-col sm="2" class="mt-3">
                            <b-form-input type="text" v-model="dpi" readonly size="sm" required></b-form-input>
                        </b-col>
                        <b-col sm="10" class="mt-3">
                            <b-form-input type="text" v-model="nombre" readonly size="sm" required></b-form-input>
                        </b-col> -->
                        <b-col sm="5" class="mt-3">
                            <label for="">Área</label>
                            <select id="asig_area2" class="form-control form-control-sm" v-model="area">
                                <option value="">Selecciona</option>
                                <option v-for="(item, index) in areas" :key="index" :value="item.id">{{item.area}}</option>
                            </select>
                        </b-col>
                        <b-col sm="5" class="mt-3">
                            <label for="">Privilegio</label>
                            <select id="asig_priv2" class="form-control form-control-sm" v-model="priv">
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
            </b-container>
            
        </div>
    </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import { minix } from '../../functions/alertas'
export default {
    name: 'ModalAgregarPrivilegios',
    props:['_dpi', '_nombre'],
    computed: {
        ...mapState(['areas', 'privilegios'])
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
        cerrar(){
            this.$emit('salir')
        },
        ranking__(id){
            let r = this.privilegios.filter(z => z.id == id )
            return r[0].rankin
        },
        async agregarAsignacion(){
            if (this.nombre == '') {
                minix({icon: 'error', mensaje: 'Sebes seleccionar un miembro', tiempo: 3000})
            }else{
                if (this.area == '') {
                    minix({icon: 'error', mensaje: 'Área es obligatorio', tiempo: 3000})
                    document.getElementById('asig_area2').focus()
                }else if (this.priv == '') {
                    minix({icon: 'error', mensaje: 'Privilegio es obligatorio', tiempo: 3000})
                    document.getElementById('asig_priv2').focus()
                }else{
                    let h = {
                        area: this.area,
                        dns_area: await this.convertir({array: 'areas', valor: 'id', parametro: this.area}),
                        privilegio: this.priv,
                        rankin_priv: await this.ranking__(this.priv),
                        dns_priv: await this.convertir({array: 'privilegios', valor: 'id', parametro: this.priv})
                    }

                    this.bolsa.push(h)

                    this.area = ''
                    this.priv = ''

                    document.getElementById('asig_area2').focus()
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
            await this.pullData({api: 'ancianos'})
            await this.$emit('actualizar_privilegios')
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
        ...mapActions(['convertir', 'saveData', 'getData', 'pullData'])
    },
    mounted() {
        this.dpi = this._dpi
        this.nombre = this._nombre
    },
}
</script>

<style>

</style>