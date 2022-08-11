<template>
    <b-container fluid>
        <form @submit.prevent="actualizarDatos">
            <b-row>
                <b-col sm="12" class="mt-3">
                    <h2>
                        Edición datos personales
                    </h2>
                </b-col>
                <b-col sm="3" class="mt-3">
                    <label for="">DPI</label>
                    <b-form-input type="text" v-model="dpi" readonly required size="sm"></b-form-input>
                </b-col>
                <b-col sm="7" class="mt-3">
                    <label for="">Nombre</label>
                    <b-form-input type="text" id="nombre_edicion_miembro" v-model="nombre" required size="sm"></b-form-input>
                </b-col>
                <b-col sm="2" class="mt-3">
                    <label for="">Sexo</label>
                    <select class="form-control form-control-sm" v-model="genero" required>
                        <option value="">Selecciona</option>
                        <option value="M">Másculino</option>
                        <option value="F">Femenino</option>
                    </select>
                </b-col>
                <b-col sm="8" class="mt-3">
                    <label for="">Dirección</label>
                    <b-form-input type="text" v-model="direccion" required size="sm"></b-form-input>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Nacionalidad</label>
                    <b-form-input type="text" v-model="nacionalidad" required size="sm"></b-form-input>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Teléfono de casa</label>
                    <b-form-input type="text" v-model="telefono" required size="sm"></b-form-input>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Teléfono móvil</label>
                    <b-form-input type="text" v-model="telefono_movil"  size="sm"></b-form-input>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Teléfono trabajo</label>
                    <b-form-input type="text" v-model="telefono_trabajo"  size="sm"></b-form-input>
                </b-col>

                
                <b-col sm="12" class="mt-3">
                    <label for="">¿Labora actualmente?</label>
                    <select class="form-control form-control-sm" v-model="labora_actualmente">
                        <option value="">Selecciona</option>
                        <option value="1">Si</option>
                        <option value="0">No</option>
                    </select>
                </b-col>

                <b-col sm="12" v-if="labora_actualmente == 1" class="mt-3">
                    <label for="">¿Nombre del lugar o empresa donde labora?</label>
                    <b-form-input type="text" v-model="nombre_empresa_donde_labora"  size="sm"></b-form-input>
                </b-col>


                <b-col sm="6" class="mt-3">
                    <label for="">En caso de emergencia llamar a:</label>
                    <b-form-input type="text" v-model="nombre_en_caso_emergencia"  size="sm"></b-form-input>
                </b-col>
                <b-col sm="6" class="mt-3">
                    <label for="">Teléfono en caso de emergencia</label>
                    <b-form-input type="text" v-model="telefono_en_caso_emergencia"  size="sm"></b-form-input>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Correo</label>
                    <b-form-input type="text" v-model="correo"  size="sm"></b-form-input>
                </b-col>

                <b-col sm="4" class="mt-3">
                    <label for="">Fecha de nacimiento</label>
                    <b-form-input type="date" v-model="fecha_de_nacimiento" required size="sm"></b-form-input>
                </b-col>
                
                <b-col sm="4" class="mt-3">
                    <label for="">Fecha de fallecimiento</label>
                    <b-form-input type="date" v-model="fecha_de_fallecimiento"  size="sm"></b-form-input>
                </b-col>

                <b-col sm="6" class="mt-3">
                    <label for="">Estado civil</label>
                    <!-- <b-form-input type="text" v-model="estado_civil" required size="sm"></b-form-input> -->
                    <select class="form-control form-control-sm" v-model="estado_civil">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in estadocivil" :key="index" :value="item.estado">{{item.estado}}</option>
                    </select>
                </b-col>

                <b-col sm="6" class="mt-3">
                    <label for="">Profesión u Oficio</label>
                    <!-- <b-form-input type="text" v-model="profesion" required size="sm"></b-form-input> -->
                    <select class="form-control form-control-sm" v-model="profesion">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in profesiones" :key="index" :value="item.id">{{item.profesion}}</option>
                    </select>
                </b-col>
                
                <b-col sm="12" class="mt-3">
                    <label for="">Ocupación</label>
                    <b-form-input type="text" v-model="ocupacion"  size="sm"></b-form-input>
                </b-col>

                <b-col sm="4" class="mt-3">
                    <label for="">Conyuge</label>
                    <b-form-input type="text" v-model="conyuge"  size="sm"></b-form-input>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Teléfono de conyuge</label>
                    <b-form-input type="text" v-model="telefono_conyuge"  size="sm"></b-form-input>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Número de hijos</label>
                    <b-form-input type="number" v-model="nohijos" size="sm"></b-form-input>
                </b-col>
                <b-col sm="12" class="mt-3 d-flex flex-row-reverse">
                    <b-button type="submit" variant="primary" size="sm">Actualizar</b-button>
                </b-col>
            </b-row>
        </form>

    </b-container>
</template>

<script>
import moment from 'moment'
import { mapActions, mapState } from 'vuex'
export default {
    name: 'EdicionDatosPersonalesForm',
    computed: {
        ...mapState(['datos_edicion_miembro', 'estadocivil', 'profesiones'])
    },
    data() {
        return {
            dpi: '',
            nombre: '',
            direccion: '',
            telefono: '',
            telefono_movil: '',
            telefono_trabajo: '',
            labora_actualmente: '',
            nombre_empresa_donde_labora: '',
            nombre_en_caso_emergencia: '',
            telefono_en_caso_emergencia: '',
            correo: '',
            fecha_de_nacimiento: '',
            genero: '',
            fecha_de_fallecimiento: '',
            estado_civil: '',
            profesion: '',
            nacionalidad: '',
            ocupacion: '',
            conyuge: '',
            telefono_conyuge: '',
            nohijos: ''
        }
    },
    methods: {
        setDatos(){
            this.dpi = this.datos_edicion_miembro.dpi
            this.nombre = this.datos_edicion_miembro.nombre
            this.direccion = this.datos_edicion_miembro.direccion
            this.telefono = this.datos_edicion_miembro.telefono
            this.telefono_movil = this.datos_edicion_miembro.telefono_movil
            this.telefono_trabajo = this.datos_edicion_miembro.telefono_trabajo
            this.labora_actualmente = this.datos_edicion_miembro.labora_actualmente
            this.nombre_empresa_donde_labora = this.datos_edicion_miembro.nombre_empresa_donde_labora
            this.nombre_en_caso_emergencia = this.datos_edicion_miembro.nombre_en_caso_emergencia
            this.telefono_en_caso_emergencia = this.datos_edicion_miembro.telefono_en_caso_emergencia
            this.correo = this.datos_edicion_miembro.correo
            this.fecha_de_nacimiento = moment(this.datos_edicion_miembro.fechadenacimiento).format('YYYY-MM-DD')
            this.genero = this.datos_edicion_miembro.genero
            this.fecha_de_fallecimiento = this.datos_edicion_miembro.fecha_de_nacimiento == null ? '' : moment(this.datos_edicion_miembro.fechadefallecimiento).format('YYYY-MM-DD')
            this.estado_civil = this.datos_edicion_miembro.estadocivil
            this.profesion = this.datos_edicion_miembro.profesion
            this.nacionalidad = this.datos_edicion_miembro.nacionalidad
            this.ocupacion = this.datos_edicion_miembro.ocupacion
            this.conyuge = this.datos_edicion_miembro.conyuge
            this.telefono_conyuge = this.datos_edicion_miembro.telefono_conyuge
            this.nohijos = this.datos_edicion_miembro.nohijos
        },
        async actualizarDatos(){
            let f = {
                api: 'miembros',
                id: this.datos_edicion_miembro.dpi,
                modo: 'param',
                pull: false,
                formulario: {
                    dpi: this.dpi.trim(),
                    nombre: this.nombre.toUpperCase().trim(),
                    direccion: this.direccion,
                    telefono: this.telefono.trim(),
                    telefono_movil: this.telefono_movil.trim(),
                    telefono_trabajo: this.telefono_trabajo.trim(),
                    labora_actualmente: this.labora_actualmente,
                    nombre_empresa_donde_labora: this.labora_actualmente == 1 ? this.nombre_empresa_donde_labora.toUpperCase().trim() : 'NO APLICA',
                    nombre_en_caso_emergencia: this.nombre_en_caso_emergencia.toUpperCase().trim(),
                    telefono_en_caso_emergencia: this.telefono_en_caso_emergencia,
                    correo: this.correo.trim(),
                    fechadenacimiento: moment(this.fecha_de_nacimiento).format('YYYY-MM-DD'),
                    genero: this.genero,
                    fechadefallecimiento: this.fecha_de_fallecimiento == '' ? null : moment(this.fecha_de_fallecimiento).format('YYYY-MM-DD'),
                    estadocivil: this.estado_civil,
                    profesion: this.profesion,
                    nacionalidad: this.nacionalidad.toUpperCase().trim(),
                    ocupacion: this.ocupacion.toUpperCase().trim(),
                    conyuge: this.conyuge.toUpperCase().trim(),
                    telefono_conyuge: this.telefono_conyuge,
                    nohijos: this.nohijos
                }
            }

            await this.updateData(f)
        },
        ...mapActions(['updateData'])
    },
    mounted() {
        this.setDatos()
        document.getElementById('nombre_edicion_miembro').focus()
    },
}
</script>

<style>

</style>