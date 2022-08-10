<template>
    <b-container fluid="">
        <form @submit.prevent="guardar" >
            <b-row v-if="mostrar_formulario == false">
                <b-col sm="12" class="mt-3">
                    <NoEncontrado :titulo="'No se registraron datos en fomulario DATOS IGLESIA'" />
                </b-col>
            </b-row>
            <b-row v-if="mostrar_formulario">
                <b-col sm="12" class="mt-3">
                    <h2>
                        Edición de datos iglesia
                    </h2>
                </b-col>

                <b-col sm="6" class="mt-3">
                    <label for="">Año de conversión</label>
                    <!-- <b-form-input type="date" v-model="fechaConversion" required size="sm"></b-form-input> -->
                    <select class="form-control form-control-sm" v-model="fechaConversion" required>
                        <option value="">Seleccionar</option>
                        <option v-for="(item, index) in 151" :key="index" :value="1899 + item">{{1899 + item}}</option>
                    </select>
                </b-col>
                <b-col sm="6" class="mt-3">
                    <label for="">Lugar de conversión</label>
                    <!-- <b-form-input type="text" v-model="lugar_conversion" required size="sm"></b-form-input> -->
                    <select class="form-control form-control-sm" required v-model="lugar_conversion">
                        <option value="">Selecciona</option>
                        <option value="IGLESIA DE JESUCRISTO PALABRA MIEL VILLA NUEVA">Iglesia de Jesucristo Palabra Miel Villa Nueva</option>
                        <option value="OTROS">Otro</option>
                    </select>
                </b-col>


                <b-col v-if="lugar_conversion == 'OTROS'" sm="12" class="mt-3">
                    <label for="">¿Cúal fue el lugar de conversión?</label>
                    <b-form-input type="text" v-model="cual_fue_lugar_de_conversion" required size="sm"></b-form-input>
                </b-col>

                <b-col sm="12" class="mt-3">
                    <label for="">¿Cual fue su congregación anterior?</label>
                    <!-- <b-form-input type="text" v-model="congregacionAnterior" size="sm"></b-form-input> -->
                    <select class="form-control form-control-sm" v-model="congregacionAnterior">
                        <option value="">Selecciona</option>
                        <option value="IGLESIA DE JESUCRISTO PALABRA MIEL">IGLESIA DE JESUCRISTO PALABRA MIEL</option>
                        <option value="OTROS">Otros</option>
                        <option value="NO APLICA">No Aplica</option>
                    </select>
                </b-col>

                 <b-col v-if="congregacionAnterior == 'IGLESIA DE JESUCRISTO PALABRA MIEL'" sm="6" class="mt-3">
                        <label for="">¿Cúal es el nombre de la iglesia Plabra Miel anterior?</label>
                        <b-form-input type="text" v-model="nombre_iglesia_anterior" required size="sm"></b-form-input>
                    </b-col>

                    <b-col v-if="congregacionAnterior == 'IGLESIA DE JESUCRISTO PALABRA MIEL'" sm="6" class="mt-3">
                        <label for="">¿Tiene carta de recomendación?</label>
                        <select class="form-control form-control-sm" v-model="cartarecomendacion">
                            <option value="">Selecciona</option>
                            <option value="1">Si</option>
                            <option value="0">No</option>
                        </select>
                    </b-col>

                     <b-col v-if="congregacionAnterior == 'OTROS'" sm="12" class="mt-3">
                        <label for="">¿Cúal es el nombre de la iglesia anterior?</label>
                        <b-form-input type="text" v-model="nombre_iglesia_anterior_otros" required size="sm"></b-form-input>
                    </b-col>


                <!-- <b-col sm="6" class="mt-3">
                    <label for="">¿Cual fue su congregación anterior?</label>
                    <b-form-input type="text" v-model="congregacionAnterior" size="sm"></b-form-input>
                </b-col> -->

                <b-col sm="12" class="mt-3">
                    <label for="">Motivo de traslado</label>
                    <b-form-input type="text" v-model="motivotraslado" size="sm"></b-form-input>
                </b-col>

                <b-col sm="6" class="mt-3">
                    <label for="">¿Es bautizado en agua?</label>
                    <select class="form-control form-control-sm" v-model="pregunta_bautizmo_agua">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col>
                <!-- <b-col v-if="pregunta_bautizmo_agua == 1" sm="12" class="mt-3">
                    <label for="">Fecha de bautizmo en agua</label>
                    <b-form-input type="date" v-model="fechaBautizmo" required size="sm"></b-form-input>
                </b-col> -->
                <b-col sm="6" class="mt-3">
                    <label for="">¿Es bautizado en Espíritu Santo?</label>
                    <select class="form-control form-control-sm" v-model="pregunta_bautizmo_espiritu">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col>
                <!-- <b-col  v-if="pregunta_bautizmo_espiritu == 1"  sm="12" class="mt-3">
                    <label for="">Fecha de bautizmo en Espíritu Santo</label>
                    <b-form-input type="date" v-model="fechaBautizmoEspiritu" required size="sm"></b-form-input>
                </b-col> -->
                <!-- <b-col sm="6" class="mt-3">
                    <label for="">¿Quienes son convertidos en su hogar?</label>
                    <b-form-input type="text" v-model="pregunta_convertidos_hogar" placeholder="Separar por comas" required size="sm"></b-form-input>
                </b-col> -->
                

                <!-- <b-col sm="6" class="mt-3">
                    <label for="">¿Se discipula?</label>
                    <select class="form-control form-control-sm" required v-model="sediscipula">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col> -->
                <!-- <b-col v-if="sediscipula == 1" sm="12" class="mt-3">
                    <label for="">¿Dónde se discipula?</label>
                    <select class="form-control form-control-sm" v-model="donde_se_discipula">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in datos_lugares_discipulado" :key="index" :value="item.id">{{item.anfitrion}}</option>
                    </select>
                </b-col>
                <b-col  v-if="sediscipula == 0 && sediscipula != ''"  sm="12" class="mt-3">
                    <label for="">¿Desea Discipularse?</label>
                    <select class="form-control form-control-sm" required v-model="afirmacionsediscipula">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col> -->
                <b-col sm="12" class="mt-3">
                    <label for="">Fecha en la que empezó a a asistir a la iglesia</label>
                    <b-form-input type="date" v-model="fechaAsistenciaIglesia" required size="sm"></b-form-input>
                </b-col>
                <!-- <b-col sm="6" class="mt-3">
                    <label for="">¿Tiene carta de recomendación?</label>
                    <select class="form-control form-control-sm" v-model="cartarecomendacion">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col> -->

                <b-col sm="12" class="mt-3">
                    <label for="">¿Tiene familia en la congregación?</label>
                    <select class="form-control form-control-sm" v-model="conocidos_congregandose">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col>
                
                <b-col  v-if="conocidos_congregandose == 1"  sm="12" class="mt-3">
                    <label for="">¿Quienes son?</label>
                    <!-- <b-form-input type="text" v-model="quienes_conocidos_se_congregan" size="sm"></b-form-input> -->
                </b-col>

                <b-col  v-if="conocidos_congregandose == 1" sm="3" class="mt-3">
                    <label for="">Parentezco</label>
                    <select class="form-control form-control-sm" id="parentezco"  v-model="parentezco">
                        <option value="">Selecciona</option>                        
                        <option value="PADRE">Padre</option>
                        <option value="MADRE">Madre</option>
                        <option value="ESPOS@">ESPOS@</option>
                        <option value="HIJ@">HIJ@</option>
                        <option value="CUÑAD@">CUÑAD@</option>
                        <option value="HERMAN@">HERMAN@</option>
                        <option value="PRIM@">Prim@</option>
                        <option value="TÍ@">TÍ@</option>
                        <option value="ABUEL@">ABUEL@</option>
                        <option value="SOBRIN@">SOBRIN@</option>
                        <option value="NOVI@">NOVI@</option>
                    </select>
                </b-col>
                <b-col  v-if="conocidos_congregandose == 1"  sm="6" class="mt-3">
                    <label for="">Nombre del familiar</label>
                    <b-form-input type="text" autocomplete="off" v-model="nombre_familiar" size="sm"></b-form-input>
                </b-col>
                <b-col  v-if="conocidos_congregandose == 1"  sm="3" class="mt-5">
                    <b-button type="button" variant="outline-success" block size="sm" @click="agrupar_familia">Agregar</b-button>
                </b-col>

                <b-col  v-if="conocidos_congregandose == 1"  sm="12" class="mt-3">
                    <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                        <thead>
                            <tr>
                                <th style="width: 15%;">
                                    Parentezco
                                </th>
                                <th style="width: 70%;">
                                    Nombre
                                </th>
                                <th style="width: 15%;text-align: center;">
                                    ...
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(item, index) in familia" :key="index">
                                <td>
                                    {{item.parentezco}}
                                </td>
                                <td>
                                    {{item.nombre}}
                                </td>
                                <td style="text-align: center;">
                                    <b-button type="button" style="font-size: 11px;margin-right: 10px;" variant="outline-primary" size="sm" @click="abrir_mod_familiares(item, index)"><i class="fas fa-pen"></i></b-button>
                                    <b-button type="button" style="font-size: 11px;" variant="outline-danger" size="sm" @click="eliminar_fila(index)"><i class="fas fa-trash"></i></b-button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    <hr>
                    
                </b-col>

                <!-- <b-col sm="6" class="mt-3">
                    <label for="">En caso de tener una necesidad. ¿Desea que lo visiten?</label>
                    <select class="form-control form-control-sm" v-model="pregunta_en_caso_necesidad">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col> -->
                <!-- <b-col sm="4" class="mt-3">
                    <label for="">¿Labora actualmente?</label>
                    <select class="form-control form-control-sm" v-model="labora">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col> -->
                <b-col sm="12" class="mt-3">
                    <label for="">Anciano que entrevistó</label>
                    <select class="form-control form-control-sm" required v-model="anciano">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in ancianos" :key="index" :value="item.id">{{item.anciano}}</option>
                        <option value="OTROS">Otros</option>
                    </select>
                </b-col>
                <b-col v-if="anciano == 'OTROS'" sm="12" class="mt-3">
                    <label for="">Nombre de quien entrevistó</label>
                    <b-form-input type="text" autocomplete="off" v-model="anciano_otros" size="sm"></b-form-input>
                </b-col>
                <b-col sm="12" class="mt-3">
                    <label for="">Observaciones</label>
                    <b-form-input type="text" autocomplete="off" v-model="comentarios" size="sm"></b-form-input>
                </b-col>
                <b-col sm="12" class="mt-3 mb-3 d-flex flex-row-reverse">
                    <b-button type="submit" variant="success" size="sm">Guardar</b-button>
                </b-col>
            </b-row>
        </form>

        <ModFamiliares v-if="mostrar_mod_familiares" :index_row="index_row" :item_row="item_row" v-on:salir="cerrar_mod_familiares" v-on:modificacion_parentezco="set_parentezco_modificacion" />

    </b-container>
</template>

<script>
import moment from 'moment'
import { mapActions, mapMutations, mapState } from 'vuex'
//import BuscarMiembro from './BuscarMiembro.vue'
import NoEncontrado from '@/components/others/NoEncontrado.vue'
import ModFamiliares from './ModFamiliares.vue'
import { minix } from '../../../functions/alertas'

export default {
    name: 'EdicionDatosIglesia',
    components:{
        NoEncontrado,
        ModFamiliares
    },
    computed: {
        ...mapState(['ancianos', 'datos_iglesia', 'datos_lugares_discipulado', 'datos_edicion_miembro'])
    },
    data() {
        return {
            modalBuscarMiembro: false,
            nombremiembro: '',
            mostrar_formulario: false,
            mostrar_mod_familiares: false,

            cui: '',
            anciano: '',
            fechaConversion: '',
            lugar_conversion: 'IGLESIA DE JESUCRISTO PALABRA MIEL VILLA NUEVA',
            pregunta_bautizmo_agua: '',
            //fechaBautizmo: '',
            pregunta_bautizmo_espiritu: '',
            //fechaBautizmoEspiritu: '',
            pregunta_convertidos_hogar: '',
            congregacionAnterior: '',
            nombre_iglesia_anterior: '',
            nombre_iglesia_anterior_otros: '',
            cual_fue_lugar_de_conversion: '',
            //sediscipula: "",
            //donde_se_discipula: '',
            //afirmacionsediscipula: '',
            fechaAsistenciaIglesia: '',
            cartarecomendacion: '',
            conocidos_congregandose: '',
            quienes_conocidos_se_congregan: '',
            motivotraslado: '',
            //pregunta_en_caso_necesidad: '',
            //labora: '',
            anciano_otros: '',
            comentarios: '',
            familia: [],
            index_row: '',
            item_row: '',
            parentezco: '',
            nombre_familiar: ''
        }
    },
    methods: {
        async guardar(){
            let d = {
                api: 'miembros/u/datos_iglesia',
                id: this.datos_edicion_miembro.dpi,
                modo: 'param',
                pull: false,
                formulario: {
                    ancianoentrevisto: this.anciano,
                    fechaconversion: this.fechaConversion,
                    lugardeconversion: this.lugar_conversion,
                    bautizmoagua: this.pregunta_bautizmo_agua,
                    //fechabautizmoagua: this.fechaBautizmo == '' ? null : this.fechaBautizmo,
                    bautizmoespiritu: this.pregunta_bautizmo_espiritu,
                    //fechabautizmoespiritu: this.fechaBautizmoEspiritu == '' ? null: this.fechaBautizmoEspiritu,
                    personasconvertidashogar: this.pregunta_convertidos_hogar,
                    congregacionanterior: this.congregacionAnterior == '' ? 'NO APLICA' : this.congregacionAnterior,
                    nombre_iglesia_anterior: this.nombre_iglesia_anterior,
                    nombre_iglesia_anterior_otros: this.nombre_iglesia_anterior_otros == '' ?  'NO APLICA' : this.nombre_iglesia_anterior_otros,
                    cual_fue_lugar_de_conversion: this.cual_fue_lugar_de_conversion,
                    //discipula: this.sediscipula,
                    //donde_se_discipula: this.donde_se_discipula,
                    //deseadisipularse: this.afirmacionsediscipula == '' ? null : this.afirmacionsediscipula,
                    fechaasistenciaiglesia: this.fechaAsistenciaIglesia,
                    cartarecomendacion: this.cartarecomendacion,
                    conocidos_congregandose: this.conocidos_congregandose,
                    quienes_conocidos_se_congregan: JSON.stringify(this.familia),
                    motivotraslado: this.motivotraslado == '' ? 'NO APLICA' : this.motivotraslado,
                    //visita: this.pregunta_en_caso_necesidad,
                    //laboraactualmente: this.labora,
                    anciano_otros: this.anciano_otros,
                    comentarios: this.comentarios
                }
            }

            await this.updateData(d)

        },
        async obtenerDatos(){
            let r = await this.getData({api: `miembros/c/datosiglesia/${this.datos_edicion_miembro.dpi}`})

            if (r.message == 'NO EXISTEN REGISTROS') {
                this.mostrar_formulario = false
            }else{
                this.mostrar_formulario = true
                this.anciano = r.ancianoentrevisto
                this.fechaConversion = r.fechaconversion
                this.congregacionAnterior = r.congregacionanterior
                this.lugar_conversion = r.lugardeconversion
                this.cual_fue_lugar_de_conversion = r.cual_fue_lugar_de_conversion
                this.nombre_iglesia_anterior = r.nombre_iglesia_anterior
                this.nombre_iglesia_anterior_otros = r.nombre_iglesia_anterior_otros
                this.pregunta_bautizmo_agua = r.bautizmoagua
                //this.fechaBautizmo = moment(r.fechabautizmoagua).format('YYYY-MM-DD')
                this.pregunta_bautizmo_espiritu = r.bautizmoespiritu
                //this.fechaBautizmoEspiritu = moment(r.fechabautizmoespiritu).format('YYYY-MM-DD')
                this.pregunta_convertidos_hogar = r.personasconvertidashogar
                //this.sediscipula = r.discipula
                //this.donde_se_discipula = r.donde_se_discipula
                this.fechaAsistenciaIglesia = moment(r.fechaasistenciaiglesia).format('YYYY-MM-DD')
                this.cartarecomendacion = r.cartarecomendacion
                this.conocidos_congregandose = r.conocidos_congregandose
                this.quienes_conocidos_se_congregan = r.quienes_conocidos_se_congregan
                this.motivotraslado = r.motivotraslado
                this.familia = JSON.parse(r.quienes_conocidos_se_congregan)
                //this.pregunta_en_caso_necesidad = r.visita
                //this.labora = r.laboraactualmente
                this.anciano_otros = r.anciano_otros
                this.comentarios = r.comentarios

            }

        },
        abrir_mod_familiares(item, index){
            this.item_row = item
            this.index_row = index
            this.mostrar_mod_familiares = true
        },
        cerrar_mod_familiares(){
            this.mostrar_mod_familiares = false
        },
        set_parentezco_modificacion(i){
            this.familia[i.index] = i.datos  
        },
        agrupar_familia(){

           if (this.parentezco == '') {
               minix({icon: 'error', mensaje: 'Parentezco debe estar lleno', tiempo: 5000})
               document.getElementById('parentezco').focus()
           }else if (this.nombre_familiar == '') {
               minix({icon: 'error', mensaje: 'Campo nombre familiar debe estar lleno', tiempo: 5000})
               document.getElementById('nombre_familiar').focus()
           }else{
               let f = {
                   nombre: this.nombre_familiar.toUpperCase().trim(),
                   parentezco: this.parentezco.toUpperCase().trim()
               }
                console.log(f)
               this.familia.push(f)
               
               this.nombre_familiar = ''
               this.parentezco = ''
           }

        },
        eliminar_fila(index){
            this.familia.splice(index, 1)
        },
        ...mapActions(['updateData', 'getData']),
        ...mapMutations(['set_datos_iglesia'])
    },
    mounted() {
        this.obtenerDatos()
    },
}
</script>

<style>
    
</style>