<template>
    <b-container fluid>
        <form @submit.prevent="guardarAsignaciones">
            <b-row v-if="tiene_privilegios == false">
                <b-col sm="12">                
                    <div class="titulo_formulario">
                        Asignar privilegios
                        <hr>
                    </div>
                </b-col>
                <b-col sm="12">                
                    <label for="">¿Tiene privilegios?</label>
                </b-col>
                <b-col sm="6">
                    <b-button type="button" variant="success" @click="consulta_tiene_privilegios(true)" size="sm" block>Si</b-button>
                </b-col>
                <b-col sm="6">
                    <b-button type="button" variant="danger" @click="consulta_tiene_privilegios(false)" size="sm" block>No</b-button>
                </b-col>
            </b-row>
            <b-row v-if="tiene_privilegios == true">
                <b-col sm="12">                
                    <div class="titulo_formulario">
                        Asignar privilegios
                        <hr>
                    </div>
                </b-col>
                <!-- <b-col sm="2" class="mt-3">
                    <b-button type="button" id="campo_buscar_miembro" size="sm" block variant="info" @click="abrirModalBuscarMiembro">Buscar</b-button>
                </b-col> -->
                <b-col sm="12" class="mt-3">
                    <label for="">Nombre</label>
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
import { mapActions, mapState, mapMutations } from 'vuex'
import { atexto, minix } from '../../functions/alertas'

import BuscarMiembro from '../../Asignacion/Seleccion/Buscar.vue'

export default {
    name: 'SeleccionarMiembro',
    computed: {
        ...mapState(['areas', 'privilegios', 'permisos', 'tipo', 'dpi_formulario', 'nombre_formulario'])
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
            bolsa: [],
            tiene_privilegios: false
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
        ranking__(id){
            let r = this.privilegios.filter(z => z.id == id)
            return r[0].rankin
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
                        rankin_priv: await this.ranking__(this.priv),
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
            await this.pullData({api: 'ancianos'})
            this.dpi = ''
            this.nombre = ''
            this.area = ''
            this.priv = ''
            this.bolsa = []
            // this.set_dpi_formulario('')
            // this.set_nombre_formulario('')

            //atexto({titulo: 'Completo!', cuerpo: 'Se ha registrado correctamente', icono: 'success'})
            this.$emit('comprobacion', 'SubeFoto')
        },
        async validarExistenciaPrivilegio(){ // PENDIENTE VALIDAR SI SE INGRESA DOBLE EN LA LISTA
            let r = await this.getData({api: `asignaciones/validacion/?dpi=${this.dpi}&privilegio=${this.priv}&area=${this.area}`})

            if (r.message == 'PUEDE ASIGNARSE') {
                this.agregarAsignacion()
            }
        },
        consulta_tiene_privilegios(valor){
            if (valor) {
                this.tiene_privilegios = true
            }else{
                // this.set_dpi_formulario('')
                // this.set_nombre_formulario('')
                //atexto({titulo: 'Completo!', cuerpo: 'Se ha registrado correctamente', icono: 'success'})
                this.$emit('comprobacion', 'SubeFoto')
            }
        },
        borrarFila(i){
            this.bolsa.splice(i, 1)
        },
        ...mapActions(['convertir', 'saveData', 'getData', 'pullData']),
        ...mapMutations(['set_dpi_formulario', 'set_nombre_formulario'])
    },
    mounted() {
        // document.getElementById('campo_buscar_miembro').focus()
        this.dpi = this.dpi_formulario
        this.nombre = this.nombre_formulario
    },
}
</script>

<style>

</style>