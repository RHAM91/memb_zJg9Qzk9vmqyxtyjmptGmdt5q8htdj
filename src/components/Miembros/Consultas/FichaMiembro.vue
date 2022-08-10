<template>
    <div class="contenedor_modal">
        <!-- <div class="cuerpo_modal"> -->
        <div class="cuerpo_modal_mod_ficha">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" @click="cerrar" size="sm">Cerrar</b-button>
            </div>
            <b-container >
                <b-row>
                    <b-col sm="3" class="mt-3">
                        <div class="foto_informacion">
                            <img :src="fotoski" style="width: 100%; height: 100%;"  @dblclick="expandirFoto" alt="">
                        </div>
                    </b-col>
                    <b-col sm="9" class="mt-3">
                        <div class="informacion_basica">
                            <div class="inombre" style="color: #2a2a2a;">
                                {{itemMiembro.nombre}} <i v-if="itemMiembro.aprobado == 1" class="fas fa-check-circle" style="color: #3C91E6"></i>
                            </div>
                            <div class="seccion_2">
                                <i class="fas fa-phone-alt"></i> &nbsp;&nbsp; {{itemMiembro.telefono_movil}}
                            </div>
                            <div class="seccion_2">
                                <i class="fas fa-address-card"></i> &nbsp;&nbsp; {{itemMiembro.dpi}}
                            </div>
                            <!-- <div class="seccion_2">
                                <i class="fas fa-user-edit" title="Creador"></i> &nbsp;&nbsp; Ingresado por: {{creador}}
                            </div> -->
                            <div class="seccion_2" style="margin-top: 135px;">
                                <b-button size="sm" variant="info" class="mb-2" style="margin-left: 15px;" @click="imprimir_reporte">
                                    <b-icon icon="printer" aria-hidden="true"></b-icon> Ficha datos generales
                                </b-button>
                                <b-button size="sm" variant="info" style="margin-left: 15px;" class="mb-2" @click="imprimir_reporte_especifico">
                                    <b-icon icon="printer" aria-hidden="true"></b-icon> Ficha datos específicos
                                </b-button>
                                <b-button size="sm" variant="warning" style="margin-left: 15px;" class="mb-2" @click="abrirModalCamara">
                                    <b-icon icon="camera" aria-hidden="true"></b-icon> Tomar foto
                                </b-button>
                                <!-- <input type="file" id="uploadfile" @change="uploadFile" ref="btnUpload"> -->
                                <b-button size="sm" variant="primary" style="margin-left: 15px;" class="mb-2" @click="abrir_modal_recotar_foto">
                                    <b-icon icon="upload" aria-hidden="true"></b-icon> Subir Foto
                                </b-button>
                            </div>
                        </div>
                    </b-col>
                    <b-col sm="12" class="mt-3">
                        <div class="separador">
                            
                        </div>
                    </b-col>
                    <b-col sm="12" class="mt-3">
                        <b-tabs content-class="mt-3" fill>
                            <b-tab title="Información personal" @click="set_submodulo('InformacionGeneral')" active></b-tab>
                            <b-tab title="Información Iglesia" @click="set_submodulo('InformacionIglesia')"></b-tab>
                            <b-tab title="Información de hijos" @click="set_submodulo('InformacionHijos')"></b-tab>
                            <b-tab v-if="itemMiembro.aprobado == 1" title="Privilegios" @click="set_submodulo('InformacionPrivilegios')"></b-tab>
                        </b-tabs>
                    </b-col>

                    <b-col sm="12" v-if="submodulo == 'InformacionGeneral'">
                        <InformacionGeneral :iMiembro="itemMiembro" :creador="creador" />
                    </b-col>
                    <b-col sm="12" v-if="submodulo == 'InformacionIglesia'">
                        <InformacionIglesia :iMiembro="itemMiembro" />
                    </b-col>
                    <b-col sm="12" v-if="submodulo == 'InformacionHijos'">
                        <InformacionHijos :iMiembro="itemMiembro" />
                    </b-col>
                    <b-col sm="12" v-if="submodulo == 'InformacionPrivilegios'">
                        <InformacionPrivilegio :iMiembro="itemMiembro" />
                    </b-col>
                </b-row>

                <TomarFoto v-if="takefoto" :iMiembro="itemMiembro" v-on:salir="cerrarModalCamara" />
                <FotoGrande v-if="expandir_foto" :iMiembro="itemMiembro.foto" v-on:salir="cerrarModalFotoGrande" />
                <RecortarFoto v-if="modal_recortar_foto" :iMiembro="itemMiembro" v-on:salir="cerrar_modal_recortar_foto" />
                <Cargando v-if="cargando" />

            </b-container>
        </div>
    </div>
</template>

<script>

import InformacionGeneral from '../Informacion/InformacionGeneral.vue'
import InformacionIglesia from '../Informacion/InformacionIglesia.vue'
import InformacionHijos from '../Informacion/InformacionHijos.vue'
import InformacionPrivilegio from '../Informacion/InformacionPrivilegios.vue'
import RecortarFoto from './RecortarFoto.vue'
import Cargando from '../../others/Cargando.vue'

import TomarFoto from './CapturaFoto.vue'
import FotoGrande from './FotoGrande.vue'
import { mapActions, mapState } from 'vuex'
import { IP, PUERTO} from '@/config/parametros'
import moment from 'moment'
import axios from 'axios'

export default {
    name: 'FichaMiembro',
    props:['dpi'],
    components: {
        InformacionGeneral,
        InformacionIglesia,
        InformacionHijos,
        InformacionPrivilegio,
        TomarFoto,
        FotoGrande,
        RecortarFoto,


        Cargando
    },
    data() {
        return {
            cargando: false,
            submodulo: 'InformacionGeneral',
            takefoto: false,
            expandir_foto: false,
            itemMiembro: {},
            creador: '',
            fotoski: '',
            modal_recortar_foto: false
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        },
        async obtenerCreador(){
            let f = await this.getData({api: `usuarios/${this.itemMiembro.quien_creo}`})
            this.creador = `${f.nombre.split(' ')[0]} ${f.apellidos}`
        },
        async obtenerDatos(){
            let f = await this.getData({api: `miembros/${this.dpi}`})
            this.itemMiembro = f
            this.fotoski = f.foto + `?${moment().format('x')}`
            await this.obtenerCreador()

        },
        set_submodulo(modulo){
            this.submodulo = modulo
        },
        abrirModalCamara(){
            this.takefoto = true
        },
        cerrarModalCamara(){
            this.takefoto = false
        },
        expandirFoto(){
            this.expandir_foto = true
        },
        subirArchivo(){
            this.$refs.btnUpload.click()
        },
        async uploadFile(event){
            let f = {
                api: 'fotos/archivos',
                evento: event.target.files[0],
                campos: [
                    {
                        campo: 'dpi',
                        payload: this.dpi
                    }
                ]
            }

            await this.sfile(f)
            await this.obtenerDatos()
        },
        cerrarModalFotoGrande(){
            this.expandir_foto = false
        },
        abrir_modal_recotar_foto(){
            this.modal_recortar_foto = true
        },
        async cerrar_modal_recortar_foto(){
            this.modal_recortar_foto = false
            await this.obtenerDatos()
        },
        async imprimir_reporte(){
            //window.open(`http://${IP}/reportes/p/membresias/ficha/imprimir_ficha.php?dpi=${this.dpi}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)

            this.cargando = true
            const url = process.env.NODE_ENV !== 'production' ? `${IP}/reportes/p` : `${IP}/reportes`

            let estampa = moment().format('x')

            let p = await axios.get(`http://${url}/membresias/ficha/imprimir_ficha.php?dpi=${this.dpi}&estampa=${estampa}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)
            if (p.status == 200) {
                await axios.get(`http://${url}/membresias/ficha/resto_ficha.php?dpi=${this.dpi}&estampa=${estampa}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)
                await axios.get(`http://${url}/membresias/ficha/unificar.php?estampa=${estampa}`)
                window.open(`http://${url}/membresias/ficha/tiraje-${estampa}.pdf`)
                this.cargando = false

            }else{
                console.log('Ocurrió un error')
            }
            
        },
        async imprimir_reporte_especifico(){
            this.cargando = true
            window.open(`http://${IP}/reportes/membresias/ficha/reporte_datos_miembros_especificos.php?dpi=${this.dpi}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)
            this.cargando = false
        },
        ...mapActions(['sfile', 'getData'])
    },
    mounted() {
        this.obtenerDatos()
    },
}
</script>

<style>

    #uploadfile{
        display: none;
    }


    .primera_informacion{
        width: 100%;
        height: auto;

        display: flex;
    }
        .foto_informacion{
            width: 200px;
            height: 250px;
            border: 1px solid #e6e6e6;
            cursor: pointer;
            object-fit: cover;
        }

        @supports(object-fit: cover){
            .foto_informacion img{
                height: 100%;
                object-fit: cover;
                object-position: center center;
            }
        }

        .inombre{
            font-size: 21px;
        }

        .informacion_basica{
            width: 100%;
            height: 150px;
        }

        .seccion_2{
            font-size: 16px;
            color: #6c6c6c;
        }
        
        .separador{
            
            border-bottom: 1px solid #e6e6e6;
        }

    .cuerpo_modal_mod_ficha{
        width: 100%;
        height: 100%;
        background-color: white;

    }
</style>