<template>
    <div class="marco">
        <b-container fluid="">
            
            <b-row v-if="mostrar_formulario == false">
                <b-col sm="12" class="mt-1">
                    <div class="titulo_formulario">
                        Datos Personales
                        <hr>
                    </div>
                </b-col>
                <b-col sm="10" class="mt-1">
                    <label for="">CUI / DPI</label>
                    <b-form-input type="text" id="cui" v-model="cui" required autocomplete="off" size="sm"></b-form-input>
                </b-col>
                <b-col sm="2" class="mt-4">
                    <b-button type="button" style="margin-top: 11px;" block variant="outline-info" size="sm" @click="validar_longitud_y_existencia">Validar</b-button>
                </b-col>
            </b-row>
            
            <form @submit.prevent="guardar" @keyup="save_storage">
                <b-row v-if="mostrar_formulario == true">
                    <b-col sm="12">
                        <!-- <b-tabs content-class="mt-3" fill>
                            <b-tab title="Registro Información general" @click="setSubModulo('InformacionGeneral')" active></b-tab>
                            <b-tab title="Registro datos Iglesia" id="modulo_1" :disabled="modulo_1"  @click="setSubModulo('DatosIglesia')" ></b-tab>
                            <b-tab title="Registro datos hijos" id="modulo_2" :disabled="modulo_2" @click="setSubModulo('DatosHijos')"></b-tab>
                        </b-tabs> -->
                        <div class="titulo_formulario">
                            Datos Personales
                            <hr>
                        </div>
                    </b-col>
                    <b-col sm="3" class="mt-3">
                        <label for="">CUI / DPI</label>
                        <b-form-input type="text" id="cui" v-model="cui" required autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="6" class="mt-3">
                        <label for="">Nombre completo</label>
                        <b-form-input type="text" v-model="nombre" id="nombre_completo_miembro" autofocus required autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="3" class="mt-3">
                        <label for="">Sexo</label>
                        <select class="form-control form-control-sm" v-model="genero" required>
                            <option value="">Selecciona</option>
                            <option value="M">Másculino</option>
                            <option value="F">Femenino</option>
                        </select>
                    </b-col>

                    <b-col sm="4" class="mt-3">
                        <label for="">Fecha de nacimiento</label>
                        <b-form-input type="date" v-model="fechaNacimiento" required autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="4" class="mt-3">
                        <label for="">Estado civil</label>
                        <select class="form-control form-control-sm" required v-model="estadocivilx">
                            <option value="">Selecciona</option>
                            <option v-for="(item, index) in estadocivil" :key="index" :value="item.estado">{{item.estado}}</option>
                        </select>
                    </b-col>


                    
                    <b-col sm="4" class="mt-3">
                        <label for="">Nacionalidad</label>
                        <b-form-input type="text" v-model="nacionalidad" required autocomplete="off" size="sm"></b-form-input>
                    </b-col>


                    <b-col sm="12" class="mt-3">
                        <label for="">Dirección</label>
                        <b-form-input type="text" v-model="direccion" required autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="4" class="mt-3">
                        <label for="">Correo Electrónico</label>
                        <b-form-input type="text" v-model="correo" required autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="4" class="mt-3">
                        <label for="">Teléfono Casa</label>
                        <b-form-input type="text" v-model="telefono" required autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="4" class="mt-3">
                        <label for="">Teléfono Móvil</label>
                        <b-form-input type="text" v-model="telefono_movil" required autocomplete="off" size="sm"></b-form-input>
                    </b-col>


                    <b-col sm="4" class="mt-3">
                        <label for="">Nombre del cónyuge</label>
                        <b-form-input type="text" v-model="conyugue" required size="sm"></b-form-input>
                    </b-col>
                   
                    <b-col sm="4" class="mt-3">
                        <label for="">Teléfono Cónyuge</label>
                        <b-form-input type="text" v-model="telefono_conyuge" required  autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <!-- <b-col sm="1" class="mt-3">
                        <label for="">DE</label>
                        <b-form-checkbox v-model="DE" @change="copiarDatos"></b-form-checkbox>
                    </b-col> -->

                    <b-col sm="4" class="mt-3">
                        <label for="">Número de hijos</label>
                        <b-form-input type="number" v-model="noHijos" required size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="12" class="mt-3">
                        <label for="">En caso de emergencia llamar a:</label>
                        <select class="form-control form-control-sm" required v-model="select_emergencia">
                            <option value="">Seleccionar</option>
                            <option value="CONYUGE">CONYUGE</option>
                            <!-- @NOTA: preguntar por estos dos registros -->
                            <option value="NUERA">NUERA</option>
                            <option value="CONCUÑ@">CONCUÑ@</option>
                            <option value="OTROS">OTROS</option>
                        </select>
                    </b-col>

                    <b-col v-if="select_emergencia == 'OTROS' || select_emergencia == 'CONCUÑ@' || select_emergencia == 'NUERA'" sm="6" class="mt-3">
                        <label for="">Nombre de emergencia:</label>
                        <b-form-input type="text" v-model="nombre_en_caso_emergencia"  autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col v-if="select_emergencia == 'OTROS' || select_emergencia == 'CONCUÑ@' || select_emergencia == 'NUERA'" sm="6" class="mt-3">
                        <label for="">Teléfono emergencia</label>
                        <b-form-input type="text" v-model="telefono_en_caso_emergencia"  autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    


                    

                    <!-- <b-col sm="12" class="mt-3">
                        <label for="">Status</label>
                        <select class="form-control form-control-sm" v-model="status" @change="MostrarFechaFallecimiento">
                            <option value="">Selecciona</option>
                            <option value="Activo">Activo</option>
                            <option value="Inactivo">Inactivo</option>
                            <option value="Fallecido">Fallecido</option>
                        </select>
                    </b-col> -->

                    <!-- <b-col sm="12" class="mt-3" v-if="mostrarCampoFechaFallecimiento">
                        <label for="">Fecha fallecimiento</label>
                        <b-form-input type="date" v-model="fechaFallecimiento"  size="sm"></b-form-input>
                    </b-col> -->

                    <b-col sm="6" class="mt-3">
                        <label for="">Profesión u Oficio</label>
                        <select class="form-control form-control-sm" required v-model="profesion">
                            <option value="">Selecciona</option>
                            <option v-for="(item, index) in profesiones" :key="index" :value="item.id">{{item.profesion}}</option>
                        </select>
                    </b-col>

                    <b-col sm="6" class="mt-3">
                        <label for="">Ocupación actual</label>
                        <b-form-input type="text" v-model="ocupacionActual" required size="sm"></b-form-input>
                    </b-col>

                    


                    <b-col sm="12" class="mt-3">
                        <label for="">¿Labora actualmente?</label>
                        <select class="form-control form-control-sm" required v-model="labora_actualmente">
                            <option value="">Selecciona</option>
                            <option value="1">Si</option>
                            <option value="0">No</option>
                        </select>
                    </b-col>

                    <b-col v-if="labora_actualmente == 1" sm="8" class="mt-3">
                        <label for="">Nombre lugar de trabajo o nombre de empresa donde labora</label>
                        <b-form-input type="text" size="sm" v-model="nombre_empresa_donde_labora" required autocomplete="off"></b-form-input>
                    </b-col>

                    <b-col v-if="labora_actualmente == 1" sm="4" class="mt-3">
                        <label for="">Teléfono Trabajo</label>
                        <b-form-input type="text" v-model="telefono_trabajo"  placeholder="Opcional" autocomplete="off" size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="6" class="mt-3">
                        <b-button type="button" size="sm" variant="outline-danger" @click="seleccionar_accion_para_limpiar(2)">Limpiar</b-button>
                    </b-col>

                    <b-col sm="6" class="mt-3 d-flex flex-row-reverse">
                        <b-button type="submit" size="sm" variant="success">Siguiente</b-button>
                    </b-col>

                </b-row>
            </form>
        </b-container>

    </div>
</template>

<script>
import { mapActions, mapMutations, mapState } from 'vuex'
import { atexto, minix, pregunta } from '../../functions/alertas'

import { IP, PUERTO } from '@/config/parametros'
import axios from 'axios'

export default {
    name: 'InformacionGeneral',
    computed:{
        ...mapState(['profesiones', 'estadocivil', 'datos_personales'])
    },
    data() {
        return {
            mostrar_formulario: false,
            mostrarCampoFechaFallecimiento: false,
            cui: '',
            nombre: '',
            genero: '',
            direccion: '',
            nacionalidad: '',
            telefono: '',
            telefono_movil: '',
            telefono_trabajo: '',
            nombre_empresa_donde_labora: '',
            nombre_en_caso_emergencia: '',
            telefono_en_caso_emergencia: '',
            correo: '',
            fechaNacimiento: '',
            fechaFallecimiento: '',
            estadocivilx: '',
            profesion: '',
            labora_actualmente: '',
            ocupacionActual: '',
            conyugue: '',
            telefono_conyuge: '',
            noHijos: 0,
            select_emergencia: ''
            
        }
    },
    methods: {
        // MostrarFechaFallecimiento(){
        //     if (this.status == 'Fallecido') {
        //         this.mostrarCampoFechaFallecimiento = true
        //     }else{
        //         this.mostrarCampoFechaFallecimiento = false
        //     }
        // },
        async guardar(){
            let f = {
                api: 'miembros',
                pull: false,
                formulario: {
                    dpi: this.cui.trim(),
                    nombre: this.nombre.toUpperCase().trim(),
                    direccion: this.direccion.toUpperCase().trim(),
                    telefono: this.telefono.toUpperCase(),
                    telefono_movil: this.telefono_movil == '' ? 'NO APLICA' : this.telefono_movil.toUpperCase(),
                    telefono_trabajo: this.telefono_trabajo == '' ? 'NO APLICA' : this.telefono_trabajo.toUpperCase(),
                    nombre_empresa_donde_labora: this.nombre_empresa_donde_labora == '' ? 'NO APLICA' : this.nombre_empresa_donde_labora.toUpperCase().trim(),
                    nombre_en_caso_emergencia: this.select_emergencia == 'CONYUGE' ? 'CONYUGE' : this.nombre_en_caso_emergencia.toUpperCase().trim(),
                    telefono_en_caso_emergencia: this.select_emergencia == 'CONYUGE' ? this.telefono_conyuge : this.telefono_en_caso_emergencia ,
                    correo: this.correo == '' ? 'NO APLICA' : this.correo,
                    fechadenacimiento: this.fechaNacimiento,
                    genero: this.genero,
                    estado: 'Activo',
                    fechadefallecimiento: this.fechaFallecimiento == '' ? null: this.fechaFallecimiento,
                    estadocivil: this.estadocivilx,
                    profesion: this.profesion,
                    labora_actualmente: this.labora_actualmente,
                    nacionalidad: this.nacionalidad.toUpperCase().trim(),
                    ocupacion: this.ocupacionActual.toUpperCase().trim(),
                    conyuge: this.conyugue == '' ? 'N/A': this.conyugue.toUpperCase().trim(),
                    telefono_conyuge: this.telefono_conyuge,
                    nohijos: this.noHijos == '' ? 0: this.noHijos
                }
            }

            let rcv = await this.saveData(f)
            
            if (rcv == 'MIEMBRO YA EXISTE') {
                console.log(rcv)
            }else{
                //await this.pullData({api: 'asignacion_entrevistas'}) /// se deshabilita debido que ya no se hará proceso de entrevista
                await this.set_dpi_formulario(this.cui)
                await this.set_nombre_formulario(this.nombre)
    
                await this.seleccionar_accion_para_limpiar(1)
                document.getElementById('cui').focus()
                this.$emit('comprobacion', 'DatosIglesia')
            }


            
        },
        save_storage(){

            let info = {
                cui: this.cui,
                nombre: this.nombre,
                direccion: this.direccion,
                telefono: this.telefono,
                telefono_movil: this.telefono_movil,
                telefono_trabajo: this.telefono_trabajo,
                nombre_empresa_donde_labora: this.nombre_empresa_donde_labora,
                nombre_en_caso_emergencia: this.nombre_en_caso_emergencia,
                telefono_en_caso_emergencia: this.telefono_en_caso_emergencia,
                correo: this.correo,
                fechaNacimiento: this.fechaNacimiento,
                genero: this.genero,
                fechaFallecimiento: this.fechaFallecimiento,
                estadocivilx: this.estadocivilx,
                profesion: this.profesion,
                labora_actualmente: this.labora_actualmente,
                nacionalidad: this.nacionalidad,
                ocupacionActual: this.ocupacionActual,
                conyugue: this.conyugue,
                telefono_conyuge: this.telefono_conyuge,
                noHijos: this.noHijos
            }

            let cadena = JSON.stringify(info)
            this.set_datos_personales(cadena)

        },
        restore_data(){
            if (this.datos_personales != '') {               
                let r = JSON.parse(this.datos_personales)
                
                this.cui = r.cui
                this.nombre = r.nombre
                this.direccion = r.direccion
                this.telefono = r.telefono
                this.telefono_movil = r.telefono_movil
                this.telefono_trabajo = r.telefono_trabajo
                this.nombre_empresa_donde_labora = r.nombre_empresa_donde_labora
                this.nombre_en_caso_emergencia = r.nombre_en_caso_emergencia
                this.telefono_en_caso_emergencia = r.telefono_en_caso_emergencia
                this.correo = r.correo
                this.fechaNacimiento = r.fechaNacimiento
                this.genero = r.genero
                this.fechaFallecimiento = r.fechaFallecimiento
                this.estadocivilx = r.estadocivilx
                this.profesion = r.profesion
                this.labora_actualmente = r.labora_actualmente
                this.nacionalidad = r.nacionalidad
                this.ocupacionActual = r.ocupacionActual
                this.conyugue = r.conyugue
                this.telefono_conyuge = r.telefono_conyuge
                this.noHijos = r.noHijos
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
        async validar_longitud_y_existencia(){
            let cui_trim = this.cui.trim()
            
            let datos = await axios.get(`http://${IP}:${PUERTO}/api/miembros/c/validacion_existencia_miembro/${cui_trim}`, this.$store.state.token)

            if (datos.data.message == 'NO REGISTROS') {
                this.mostrar_formulario = true
                
            }else if(datos.data.message == 'MIEMBRO YA EXISTE.'){
                atexto({titulo: 'Invalido!', cuerpo: 'Miembro ya fue registrado', icono: 'error'})
            }else if (datos.data.message == 'EL NUMERO DE DPI O CUI DEBE SER DE 13 O 9 CARACTERES') {
                atexto({titulo: 'Atención!', cuerpo: datos.data.message, icono: 'info'})
            }
            
        },
        limpiar(){
            this.cui = ''
            this.nombre = ''
            this.direccion = ''
            this.telefono = ''
            this.telefono_movil = ''
            this.telefono_trabajo = ''
            this.nombre_empresa_donde_labora = ''
            this.nombre_en_caso_emergencia = ''
            this.telefono_en_caso_emergencia = ''
            this.correo = ''
            this.fechaNacimiento = ''
            this.status = ''
            this.fechaFallecimiento = ''
            this.genero = ''
            this.estadocivilx = ''
            this.profesion = ''
            this.labora_actualmente = ''
            this.nacionalidad = ''
            this.ocupacionActual = ''
            this.conyugue = ''
            this.telefono_conyuge = ''
            this.noHijos = ''

            this.set_datos_personales('')
            document.getElementById('cui').focus()
        },
        ...mapActions(['saveData', 'pullData', 'getData']),
        ...mapMutations(['set_datos_personales', 'set_dpi_formulario', 'set_nombre_formulario'])
    },
    mounted() {
        document.getElementById('cui').focus()
        this.restore_data()
    }
}
</script>

<style>

</style>