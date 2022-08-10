<template>
    <div class="marco_overflow">
        <b-container fluid>
            <b-row v-if="mostrar_datos_iglesia">
                <b-col sm="12" class="mt-3">
                    <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                        <thead>
                            <tr>
                                <th>
                                    Rubro
                                </th>
                                <th>Datos</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td style="width: 20%;">
                                    Entrevista realizada por
                                </td>
                                <td style="width: 80%;">
                                    {{anciano}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Año de conversión
                                </td>
                                <td style="width: 80%;">
                                    {{recv.fechaconversion}}
                                </td>
                            </tr>
                            <tr v-if="recv.lugardeconversion == 'IGLESIA DE JESUCRISTO PALABRA MIEL VILLA NUEVA'">
                                <td style="width: 20%;">
                                    Lugar de conversión
                                </td>
                                <td style="width: 80%;">
                                    {{recv.lugardeconversion}}
                                </td>
                            </tr>
                            <tr v-if="recv.lugardeconversion == 'otros'">
                                <td style="width: 20%;">
                                    Lugar de conversión
                                </td>
                                <td style="width: 80%;">
                                    {{recv.cual_fue_lugar_de_conversion}}
                                </td>
                            </tr>
                            <tr >
                                <td style="width: 20%;">
                                    Congregación anterior
                                </td>
                                <td style="width: 80%;">
                                    {{recv.congregacionanterior}}
                                </td>
                            </tr>
                            <tr v-if="recv.congregacionanterior == 'IGLESIA DE JESUCRISTO PALABRA MIEL'">
                                <td style="width: 20%;">
                                    Iglesia Palabra Miel anterior
                                </td>
                                <td style="width: 80%;">
                                    {{recv.nombre_iglesia_anterior}}
                                </td>
                            </tr>
                            <tr v-if="recv.congregacionanterior == 'OTROS'">
                                <td style="width: 20%;">
                                    Iglesia anterior
                                </td>
                                <td style="width: 80%;">
                                    {{recv.nombre_iglesia_anterior_otros}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Carta de recomendación
                                </td>
                                <td style="width: 80%;">
                                    {{recv.cartarecomendacion | evalue}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Motivo traslado
                                </td>
                                <td style="width: 80%;">
                                    {{recv.motivotraslado}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Bautizmo en agua
                                </td>
                                <td style="width: 80%;">
                                    {{recv.bautizmoagua | evalue}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Bautizmo en Espiritu Santo
                                </td>
                                <td style="width: 80%;">
                                    {{recv.bautizmoespiritu | evalue}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Fecha inicio asistencia a iglesia
                                </td>
                                <td style="width: 80%;">
                                    {{recv.fechaasistenciaiglesia | nfecha}} **** | ****  {{asistenciaactual}} años
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Familia en la congregación
                                </td>
                                <td style="width: 80%;">
                                    {{recv.conocidos_congregandose | evalue}}
                                </td>
                            </tr>
                            <tr v-if="recv.conocidos_congregandose == 1">
                                <td style="width: 20%;">
                                    Familiares
                                </td>
                                <td style="width: 80%;">
                                    <b-button type="button" variant="outline-primary" style="font-size: 10px;" size="sm" @click="abrir_modal_ver_familiares">Ver</b-button>
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Comentarios
                                </td>
                                <td v-if="recv.comentarios == '' || recv.comentarios == null" style="width: 80%;">
                                    -- SIN COMENTARIOS --
                                </td>
                                <td v-else style="width: 80%;">
                                    {{recv.comentarios}}
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </b-col>
            </b-row> 
            <NoEncontrado v-if="mostrar_datos_iglesia == false" :titulo="'No se encontraron registros'" />
            <ModalVerFamiliares v-if="modal_ver_familiares" :familiares="familiares" v-on:salir="cerrar_modal_ver_familiares" />
            <ModalRegistroDatosIglesia v-if="modal_registro_datos_iglesia" v-on:salir="cerrar_modal_registro_datos_iglesia" />

            <div v-if="btn_agregar_datos" class="btn_agregar_formulario" @click="abrir_modal_registro_datos_iglesia">
                <i class="fas fa-plus"></i>
            </div>

        </b-container>
    </div>
</template>

<script>
import moment from 'moment'
import { mapActions, mapMutations } from 'vuex'

import NoEncontrado from '@/components/others/NoEncontrado.vue'
import ModalVerFamiliares from './ModalVerFamiliares.vue'
import ModalRegistroDatosIglesia from './ModalRegistroDatosIglesia.vue'
export default {
    name: 'FichaInformacionIglesia',
    props: ['iMiembro'],
    components: {
        NoEncontrado,
        ModalVerFamiliares,
        ModalRegistroDatosIglesia
    },
    filters:{
        nfecha: function(val){
            return moment(val).format('DD/MM/YYYY')
        },
        evalue: function(val){
            return val == 1 ? 'SI' : 'NO'
        }
    },
    data() {
        return {
            mostrar_datos_iglesia: false,
            modal_ver_familiares: false,
            btn_agregar_datos: false,
            modal_registro_datos_iglesia: false,
            asistenciaactual: 0,
            nProfesion: '',
            recv: {},
            familiares: [],
            anciano: '',
            
        }
    },
    methods: {
        async convertirProfesion(id, otros){

            if (id == '' || id == null || id == undefined || id == 'OTROS') {
                this.anciano = otros
            }else{
                let x = await this.convertir({array: 'ancianos', valor: 'id', parametro: id})
                this.anciano = x[0].anciano
            }

        },
        async consultarDatosIglesia(){
            
            let f = await this.getData({api: `miembros/c/datosiglesia/${this.iMiembro.dpi}`})
        
            if (f.message) {
                if (f.message == 'NO EXISTEN REGISTROS') {
                    this.mostrar_datos_iglesia = false
                    this.btn_agregar_datos = true
                }
            }else{
                this.mostrar_datos_iglesia = true
                this.recv = f
                this.familiares = this.recv.quienes_conocidos_se_congregan == '' ? [] : JSON.parse(this.recv.quienes_conocidos_se_congregan)
                this.convertirProfesion(f.ancianoentrevisto, f.anciano_otros)
                this.btn_agregar_datos = false
            }
            
            await this.calcularAsistencia()

        },
        calcularAsistencia(){
            const n = moment(this.recv.fechaasistenciaiglesia)
            const hoy = moment()
            this.asistenciaactual = hoy.diff(n, 'years')
        },
        abrir_modal_ver_familiares(){
            this.modal_ver_familiares = true
        },
        cerrar_modal_ver_familiares(){
            this.modal_ver_familiares = false
        },
        abrir_modal_registro_datos_iglesia(){
            this.modal_registro_datos_iglesia = true
            this.set_dpi_formulario(this.iMiembro.dpi)
            this.set_nombre_formulario(this.iMiembro.nombre)
        },
        async cerrar_modal_registro_datos_iglesia(){
            this.modal_registro_datos_iglesia = false
            this.set_dpi_formulario('')
            this.set_nombre_formulario('')
            await this.consultarDatosIglesia()
        },
        ...mapActions(['convertir', 'getData']),
        ...mapMutations(['set_dpi_formulario','set_nombre_formulario'])
    },
    mounted() {
        this.consultarDatosIglesia()
    },
}
</script>

<style>
    .marco_overflow{
        height: calc(100vh - 400px);
        overflow: auto;
    }
    
    .btn_agregar_formulario{
        width: 45px;
        height: 45px;
        position: fixed;
        bottom: 20px;
        right: 20px;
        border-radius: 50%;
        transition: .4s ease;
        background-color: #3083DC;
        color: white;
        display: flex;
        justify-content: center;
        align-items: center;      
        cursor: pointer; 
        user-select: none;
    }
        .btn_agregar_formulario:hover{
            background-color: #94C1E1;
        }
    
</style>