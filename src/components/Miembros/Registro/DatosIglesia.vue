<template>
    <div class="marco">

        <b-container fluid="">
            <form @submit.prevent="guardar" @keyup="save_storage">
                <b-row>
                    <b-col sm="12">
                        <div class="titulo_formulario">
                            Datos Iglesia
                            <hr>
                        </div>
                    </b-col>
                    <b-col sm="4" class="mt-3">
                        <label for="">CUI miembro</label>
                        <b-form-input type="text" id="cuidatosiglesia" v-model="cui" placeholder="F2 Para buscar" readonly autocomplete="off" @keyup.113="abrirModalBuscarMiembro" required size="sm"></b-form-input>
                    </b-col>
                    <b-col sm="8" class="mt-3">
                        <label for="">Nombre</label>
                        <b-form-input type="text" id="cuidatosiglesia" v-model="nombremiembro" readonly autocomplete="off" required size="sm"></b-form-input>
                    </b-col>
                    <!-- <b-col sm="6" class="mt-3">
                        <label for="">Nombre del miembro</label>
                        <b-form-input type="text" readonly v-model="nombremiembro" size="sm"></b-form-input>
                    </b-col> -->
                    <!-- <b-col sm="4" class="mt-3">
                        <label for="">Anciano que entrevistó</label>
                        <select class="form-control form-control-sm" id="seleccionanciano" required v-model="anciano">
                            <option value="">Selecciona</option>
                            <option v-for="(item, index) in ancianos" :key="index" :value="item.id">{{item.anciano}}</option>
                        </select>
                    </b-col> -->
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

                    <!-- SE SOLICITÓ QUE AL MOMENTO DE SELECCIONAR 'IGLESIA DE JESUCRISTO PALABRA MIEL VILLA NUEVA' YA NO SE MOSTRARA LA PREGUNTA ¿CUAL FUE SU CONGREGACIÓN ANTERIOR? -->

                    <b-col v-if="lugar_conversion == 'OTROS'" sm="12" class="mt-3">
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
                        <label for="">¿Cúal es el nombre de la iglesia Palabra Miel anterior?</label>
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
                        <b-form-input type="text" v-model="nombre_iglesia_anterior_otros" size="sm"></b-form-input>
                    </b-col>


                     <b-col v-if="congregacionAnterior == 'OTROS'" sm="12" class="mt-3">
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

                    <b-col sm="12" class="mt-4">
                        <label for="">Fecha en la que empezó a a asistir a la iglesia</label>
                        <b-form-input type="date" v-model="fechaAsistenciaIglesia" required size="sm"></b-form-input>
                    </b-col>

                


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

                    <b-col v-if="conocidos_congregandose == 1" sm="3" class="mt-3">
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
                    <b-col v-if="conocidos_congregandose == 1" sm="7" class="mt-3">
                        <label for="">Nombre del familiar</label>
                        <b-form-input type="text" id="nombre_familiar" v-model="nombre_familiar" placeholder="Separar por comas"  size="sm"></b-form-input>
                    </b-col>

                    <b-col v-if="conocidos_congregandose == 1" sm="2" class="mt-5">
                        <b-button type="button" block variant="outline-success" size="sm" @click="agrupar_familia">Agregar</b-button>
                    </b-col>

                    <b-col v-if="conocidos_congregandose == 1" sm="12" class="mt-3">
                        <table class="table table-striped table-sm table-bordered" style="font-size: 11px">
                            <thead>
                                <tr>
                                    <th style="width: 75%;">
                                        Nombre
                                    </th>
                                    <th style="width: 15%;text-align: center;">
                                        Parentezco
                                    </th>
                                    <th style="width: 10%;text-align: center;">
                                        ...
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in lista_parientes_convertidos" :key="index">
                                    <td>
                                        {{item.nombre}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.parentezco}}
                                    </td>
                                    <td style="text-align: center;">
                                        <b-button type="button" variant="outline-danger" style="font-size: 11px;" size="sm" @click="eliminar_fila(index)"><i class="fas fa-trash"></i></b-button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </b-col>

                    <b-col v-if="conocidos_congregandose == 1" sm="12" class="mt-3">
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
                        <b-form-input type="text" v-model="anciano_otros" size="sm"></b-form-input>
                    </b-col>
                    <b-col sm="12" class="mt-3">
                        <label for="">Observaciones</label>
                        <b-form-input type="text" v-model="comentarios"  size="sm"></b-form-input>
                    </b-col>
                    <b-col sm="6" class="mt-3">
                        <b-button type="button" variant="outline-danger" size="sm" @click="seleccionar_accion_para_limpiar(2)">Limpiar</b-button>
                    </b-col>
                    <b-col sm="6" class="mt-3 d-flex flex-row-reverse">
                        <b-button type="submit" variant="success" size="sm">Siguiente</b-button>
                    </b-col>
                </b-row>
            </form>
            <BuscarMiembro v-if="modalBuscarMiembro" v-on:salir="cerrarModalBuscarMiembro" v-on:seleccion="insertarDato" />

        </b-container>
    </div>
</template>

<script>
import moment from 'moment'
import { mapActions, mapMutations, mapState } from 'vuex'
import { minix, pregunta } from '../../functions/alertas'
import BuscarMiembro from './BuscarMiembro.vue'

export default {
    name: 'DatosIglesia',
    computed: {
        ...mapState(['ancianos', 'datos_iglesia', 'datos_lugares_discipulado', 'dpi_formulario', 'nombre_formulario'])
    },
    components:{
        BuscarMiembro
    },
    data() {
        return {
            modalBuscarMiembro: false,
            nombremiembro: '',

            cui: '',
            anciano: '',
            fechaConversion: moment(Date.now()).format('YYYY'),
            lugar_conversion: 'IGLESIA DE JESUCRISTO PALABRA MIEL VILLA NUEVA',
            cual_fue_lugar_de_conversion: '',
            pregunta_bautizmo_agua: '',
            //fechaBautizmo: '',
            pregunta_bautizmo_espiritu: '',
            //fechaBautizmoEspiritu: '',
            pregunta_convertidos_hogar: '',
            congregacionAnterior: '',
            nombre_iglesia_anterior: '',
            nombre_iglesia_anterior_otros: '',
            //sediscipula: "",
            //donde_se_discipula: '',
            //afirmacionsediscipula: '',
            fechaAsistenciaIglesia: '',
            cartarecomendacion: '',
            conocidos_congregandose: '',
            //quienes_conocidos_se_congregan: '',
            motivotraslado: '',
            //pregunta_en_caso_necesidad: '',
            //labora: '',
            anciano_otros: '',
            comentarios: '',
            parentezco: '',
            nombre_familiar: '',
            lista_parientes_convertidos: []
        }
    },
    methods: {
        async guardar(){
            let d = {
                api: 'miembros/datosiglesia',
                pull: false,
                formulario: {
                    dpi: this.cui,
                    ancianoentrevisto: this.anciano,
                    fechaconversion: this.fechaConversion,
                    lugardeconversion: this.lugar_conversion.toUpperCase().trim(),
                    cual_fue_lugar_de_conversion: this.cual_fue_lugar_de_conversion == '' ? 'NO APLICA' : this.cual_fue_lugar_de_conversion.toUpperCase().trim(),
                    bautizmoagua: this.pregunta_bautizmo_agua,
                    //fechabautizmoagua: this.fechaBautizmo == '' ? null : this.fechaBautizmo,
                    bautizmoespiritu: this.pregunta_bautizmo_espiritu,
                    //fechabautizmoespiritu: this.fechaBautizmoEspiritu == '' ? null: this.fechaBautizmoEspiritu,
                    personasconvertidashogar: this.pregunta_convertidos_hogar,
                    congregacionanterior: this.congregacionAnterior == '' ? 'NO APLICA' : this.congregacionAnterior.toUpperCase().trim(), 
                    nombre_iglesia_anterior: this.nombre_iglesia_anterior.toUpperCase().trim(),
                    nombre_iglesia_anterior_otros: this.nombre_iglesia_anterior_otros == '' ? 'NO APLICA' : this.nombre_iglesia_anterior_otros.toUpperCase().trim(),
                    //discipula: this.sediscipula,
                    //donde_se_discipula: this.donde_se_discipula,
                    //deseadisipularse: this.afirmacionsediscipula == '' ? null : this.afirmacionsediscipula,
                    fechaasistenciaiglesia: this.fechaAsistenciaIglesia,
                    cartarecomendacion: this.cartarecomendacion == '' ? 0: this.cartarecomendacion,
                    conocidos_congregandose: this.conocidos_congregandose,
                    quienes_conocidos_se_congregan: JSON.stringify(this.lista_parientes_convertidos),
                    motivotraslado: this.motivotraslado == '' ? 'NO APLICA' : this.motivotraslado.toUpperCase().trim(),
                    //visita: this.pregunta_en_caso_necesidad,
                    //laboraactualmente: this.labora,
                    anciano_otros: this.anciano_otros,
                    comentarios: this.comentarios
                }
            }

            await this.saveData(d)
            document.getElementById('cuidatosiglesia').focus()
            await this.seleccionar_accion_para_limpiar(1)
            this.$emit('comprobacion', 'DatosHijos')

        },
        cerrarModalBuscarMiembro(){
            this.modalBuscarMiembro = false
        },
        abrirModalBuscarMiembro(){
            this.modalBuscarMiembro = true
        },
        insertarDato(i){
            this.cui = i.dpi
            this.nombremiembro = i.nombre
            document.getElementById('cuidatosiglesia').focus()
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
   
               this.lista_parientes_convertidos.push(f)
               
               this.nombre_familiar = ''
               this.parentezco = ''
           }

        },
        eliminar_fila(index){
            this.lista_parientes_convertidos.splice(index, 1)
        },
        save_storage(){

            let info = {
                cui: this.cui,
                nombremiembro: this.nombremiembro,
                anciano: this.anciano,
                fechaConversion: this.fechaConversion,
                lugar_conversion: this.lugar_conversion,
                cual_fue_lugar_de_conversion: this.cual_fue_lugar_de_conversion,
                pregunta_bautizmo_agua: this.pregunta_bautizmo_agua,
                //fechaBautizmo: this.fechaBautizmo,
                pregunta_bautizmo_espiritu: this.pregunta_bautizmo_espiritu,
                //fechaBautizmoEspiritu: this.fechaBautizmoEspiritu,
                pregunta_convertidos_hogar: this.pregunta_convertidos_hogar,
                congregacionAnterior: this.congregacionAnterior,
                nombre_iglesia_anterior: this.nombre_iglesia_anterior,
                nombre_iglesia_anterior_otros: this.nombre_iglesia_anterior_otros,
                //sediscipula: this.sediscipula,
                //donde_se_discipula: this.donde_se_discipula,
                //afirmacionsediscipula: this.afirmacionsediscipula,
                fechaAsistenciaIglesia: this.fechaAsistenciaIglesia,
                cartarecomendacion: this.cartarecomendacion,
                conocidos_congregandose: this.conocidos_congregandose,
                quienes_conocidos_se_congregan: JSON.stringify(this.lista_parientes_convertidos),
                motivotraslado: this.motivotraslado == '' ? 'NO APLICA' : this.motivotraslado.toUpperCase().trim(),
                //pregunta_en_caso_necesidad: this.pregunta_en_caso_necesidad,
                //labora: this.labora,
                anciano_otros: this.anciano_otros,
                comentarios: this.comentarios
            }

            let cadena = JSON.stringify(info)
            this.set_datos_iglesia(cadena)

        },
        restore_data(){
            if (this.datos_iglesia != '') {               
                let r = JSON.parse(this.datos_iglesia)
                
                this.cui = r.cui
                this.nombremiembro = r.nombremiembro
                this.anciano = r.anciano
                this.fechaConversion = r.fechaConversion
                this.lugar_conversion = r.lugar_conversion
                this.cual_fue_lugar_de_conversion = r.cual_fue_lugar_de_conversion
                this.pregunta_bautizmo_agua = r.pregunta_bautizmo_agua
                //this.fechaBautizmo = r.fechaBautizmo
                this.pregunta_bautizmo_espiritu = r.pregunta_bautizmo_espiritu
                //this.fechaBautizmoEspiritu = r.fechaBautizmoEspiritu
                this.pregunta_convertidos_hogar = r.pregunta_convertidos_hogar
                this.congregacionAnterior = r.congregacionAnterior
                this.nombre_iglesia_anterior = r.nombre_iglesia_anterior
                this.nombre_iglesia_anterior_otros = r.nombre_iglesia_anterior_otros
                //this.sediscipula = r.sediscipula
                //this.donde_se_discipula = r.donde_se_discipula
                //this.afirmacionsediscipula = r.afirmacionsediscipula
                this.fechaAsistenciaIglesia = r.fechaAsistenciaIglesia
                this.cartarecomendacion = r.cartarecomendacion
                this.conocidos_congregandose = r.conocidos_congregandose
                this.lista_parientes_convertidos = JSON.parse(r.quienes_conocidos_se_congregan)
                this.motivotraslado = r.motivotraslado
                //this.pregunta_en_caso_necesidad = r.pregunta_en_caso_necesidad
                //this.labora = r.labora
                this.anciano_otros = r.anciano_otros
                this.comentarios = r.comentarios
            }


        },
        seleccionar_accion_para_limpiar(seleccion){
            if (seleccion == 1) {
                
                this.limpiar()
            }else{
                pregunta({titulo: 'Seguro que deseas limpiarlo?', texto: 'Esta acción no se puede revertir', afirmacion: 'Si, limpiarlo!'}, async (i) =>{

                if (i) {
                    this.limpiar()
                }
            })
            }
        },
        limpiar(){

                this.cui = ''
                this.nombremiembro = ''
                this.anciano = ''
                this.fechaConversion = ''
                this.lugar_conversion = 'Iglesia de Jesucristo Palabra Miel Villa Nueva'
                this.cual_fue_lugar_de_conversion = ''
                this.pregunta_bautizmo_agua = ''
                //this.fechaBautizmo = ''
                this.pregunta_bautizmo_espiritu = ''
                //this.fechaBautizmoEspiritu = ''
                this.pregunta_convertidos_hogar = ''
                this.congregacionAnterior = ''
                this.nombre_iglesia_anterior = ''
                this.nombre_iglesia_anterior_otros = ''
                //this.sediscipula = ''
                //this.donde_se_discipula = ''
                this.afirmacionsediscipula = ''
                this.fechaAsistenciaIglesia = ''
                this.cartarecomendacion = ''
                this.conocidos_congregandose = ''
                this.lista_parientes_convertidos = []
                this.motivotraslado = ''
                //this.pregunta_en_caso_necesidad = ''
                //this.labora = ''
                this.anciano_otros = ''
                this.comentarios = ''
                this.set_datos_iglesia('')
                //this.set_dpi_formulario('')
                //this.set_nombre_formulario('')
            
        },
        ...mapActions(['saveData']),
        ...mapMutations(['set_datos_iglesia', 'set_dpi_formulario', 'set_nombre_formulario'])
    },
    mounted() {
        document.getElementById('cuidatosiglesia').focus()
        this.restore_data()
        this.cui = this.dpi_formulario
        this.nombremiembro = this.nombre_formulario
    },
}
</script>

<style>
    
</style>