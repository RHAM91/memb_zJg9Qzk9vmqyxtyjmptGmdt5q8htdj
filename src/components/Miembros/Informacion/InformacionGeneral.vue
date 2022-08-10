<template>
    <div class="marco_overflow" >
        <b-container fluid>
            <b-row>
                <b-col sm="12" class="mt-3">
                    <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                        <thead>
                            <tr>
                                <th>
                                    Rubros
                                </th>
                                <th>Datos</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td style="width: 20%;">
                                    DPI
                                </td>
                                <td style="width: 80%;">
                                    {{recv.dpi}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Sexo
                                </td>
                                <td style="width: 80%;">
                                    {{recv.genero | genero}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Dirección
                                </td>
                                <td style="width: 80%;">
                                    {{recv.direccion}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Nacionalidad
                                </td>
                                <td style="width: 80%;">
                                    {{recv.nacionalidad}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Teléfono casa
                                </td>
                                <td style="width: 80%;">
                                    {{recv.telefono | campoNull}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Teléfono móvil
                                </td>
                                <td style="width: 80%;">
                                    {{recv.telefono_movil | campoNull}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Teléfono trabajo
                                </td>
                                <td style="width: 80%;">
                                    {{recv.telefono_trabajo | campoNull}}
                                </td>
                            </tr>

                            <tr>
                                <td style="width: 20%;">
                                    ¿Labora actualmente?
                                </td>
                                <td style="width: 80%;">
                                    {{recv.labora_actualmente | labora}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Lugar de trabajo u empresa
                                </td>
                                <td style="width: 80%;">
                                    {{recv.nombre_empresa_donde_labora}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    En caso de emergencia
                                </td>
                                <td style="width: 80%;">
                                    {{recv.nombre_en_caso_emergencia}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Teléfono emergencia
                                </td>
                                <td style="width: 80%;">
                                    {{recv.telefono_en_caso_emergencia}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Correo
                                </td>
                                <td style="width: 80%;">
                                    {{recv.correo}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Fecha de nacimiento
                                </td>
                                <td style="width: 80%;">
                                    {{recv.fechadenacimiento | nfecha}} - {{edadactual}} años
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Estado
                                </td>
                                <td style="width: 80%;">
                                    {{recv.estado}}
                                </td>
                            </tr>
                            <tr v-if="recv.estado == 'Fallecido'">
                                <td style="width: 20%;">
                                    Fecha Fallecimiento
                                </td>
                                <td style="width: 80%;">
                                    {{fechadefallecimiento}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Estado civil
                                </td>
                                <td style="width: 80%;">
                                    {{recv.estadocivil}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Profesión u oficio
                                </td>
                                <td style="width: 80%;">
                                    {{nProfesion}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Ocupación actual
                                </td>
                                <td style="width: 80%;">
                                    {{recv.ocupacion}}
                                </td>
                            </tr>
                            <tr v-if="recv.estadocivil == 'Casado'">
                                <td style="width: 20%;">
                                    Cónyuge
                                </td>
                                <td style="width: 80%;">
                                    {{recv.conyuge}}
                                </td>
                            </tr>
                            <tr v-if="recv.estadocivil == 'Casado'">
                                <td style="width: 20%;">
                                    Teléfono cónyuge
                                </td>
                                <td style="width: 80%;">
                                    {{recv.telefono_conyuge}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Número de hijos
                                </td>
                                <td style="width: 80%;">
                                    {{recv.nohijos | nonhijos}}
                                </td>
                            </tr>
                            <tr>
                                <td style="width: 20%;">
                                    Ingresado por
                                </td>
                                <td style="width: 80%;">
                                    {{creador}}
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </b-col>
            </b-row>
        </b-container>

    </div>
</template>

<script>
import moment from 'moment'
import { mapActions, mapState } from 'vuex'

export default {
    name: 'FichaInformacionGeneral',
    props: ['iMiembro', 'creador'],
    filters:{
        nfecha: function(val){
            return moment(val).format('DD/MM/YYYY')
        },
        nonhijos: function(val){
            return val == 0 ? '-- No tiene --' : val
        },
        genero: function(val){
            return val == 'M' ? 'MASCULINO' : 'FEMENINO'
        },
        campoNull: function(val){
            if (val == '' || val == null) {
                return 'N/A'
            }else{
                return val
            }
        },
        labora: function(val){
            return val == 1 ? 'SI' : 'NO'
        }
    },
    data() {
        return {
            edadactual: 0,
            nProfesion: '',
            recv: {}
        }
    },
    methods: {
        
        calcularEdad(){
            const nacimiento = moment(this.recv.fechadenacimiento)
            const hoy = moment()
            this.edadactual = hoy.diff(nacimiento, 'years')
        },
        async convertirProfesion(id){
            let x = await this.convertir({array: 'profesiones', valor: 'id', parametro: id})
            this.nProfesion = x[0].profesion

        },
        async obtenerDatos(){
            
            let f = await this.getData({api: `miembros/${this.iMiembro.dpi}`})
            this.recv = f

            await this.convertirProfesion(f.profesion)
            await this.calcularEdad()

        },
        ...mapActions(['convertir', 'getData'])
    },
    mounted() {
        
        setTimeout(() => {
            this.obtenerDatos()
        }, 1000);
    },
}
</script>

<style>

</style>