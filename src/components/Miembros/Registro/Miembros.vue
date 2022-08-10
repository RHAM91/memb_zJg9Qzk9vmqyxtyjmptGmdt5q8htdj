<template>
    <b-container fluid="">
        <b-row>
            <b-col sm="12" class="mt-3">
                <h1>
                    Miembros
                </h1>
            </b-col>

             <b-col sm="12">
                <!-- <b-tabs content-class="mt-3" fill>
                    <b-tab title="Registro Información general" @click="setSubModulo('InformacionGeneral')" active></b-tab>
                    <b-tab title="Registro datos Iglesia" id="modulo_1" :disabled="modulo_1"  @click="setSubModulo('DatosIglesia')" ></b-tab>
                    <b-tab title="Registro datos hijos" id="modulo_2" :disabled="modulo_2" @click="setSubModulo('DatosHijos')"></b-tab>
                </b-tabs> -->
                
            </b-col>
           
            
            <b-col sm="12" v-if="submodulo == 'InformacionGeneral'">
                <InformacionGeneral v-on:comprobacion="activacion" />
            </b-col>

            <b-col sm="12" v-if="submodulo == 'DatosIglesia'">
                <DatosIglesia v-on:comprobacion="activacion"/>
            </b-col>

            <b-col sm="12" v-if="submodulo == 'DatosHijos'">
                <DatosHijos v-on:comprobacion="activacion" />
            </b-col>

            <b-col sm="12" v-if="submodulo == 'AsignacionPrivilegios'">
                <APrivilegios v-on:comprobacion="activacion" />
            </b-col>
            <b-col sm="12" v-if="submodulo == 'SubeFoto'">
                <SubirFoto v-on:comprobacion="activacion" />
            </b-col>
        </b-row>

        <div class="btn_menu_formularios">
            <button type="button" class="in_btn_menu_formularios" id="btn_datos_personales" @click="setSubModulo('InformacionGeneral')">DP</button>
            <button type="button" class="in_btn_menu_formularios" id="btn_datos_iglesia" @click="setSubModulo('DatosIglesia')">DI</button>
            <button type="button" class="in_btn_menu_formularios" id="btn_datos_hijos" @click="setSubModulo('DatosHijos')">DH</button>
            <b-button type="button" variant="danger" size="sm" :disabled="dpi_formulario == ''" @click="canelarTodo"><i class="fas fa-ban"></i></b-button>
        </div>

    </b-container>
</template>

<script>

import InformacionGeneral from './InformacionGeneral.vue'
import DatosIglesia from './DatosIglesia.vue'
import DatosHijos from './DatosHijos.vue'
import APrivilegios from './AsignacionPrivilegios.vue'
import SubirFoto from './SubirFoto.vue'

import { pregunta } from '../../functions/alertas'
import { mapActions, mapMutations, mapState } from 'vuex'

export default {
    name: 'Miembros',
    computed: {
        ...mapState(['dpi_formulario'])
    },
    components: {
        InformacionGeneral,
        DatosIglesia,
        DatosHijos,
        APrivilegios,
        SubirFoto
    },
    data() {
        return {
            submodulo: 'InformacionGeneral',
        }
    },
    methods: {
        setSubModulo(sub){
            this.submodulo = sub
        },
        activacion(v){
            this.submodulo = v
        },
        async canelarTodo(){        
            pregunta({titulo: 'Seguro que no deseas continuar?', texto: 'Se borrará todo lo que ya se ha ingresado', afirmacion: 'Si!, continuar'}, async (i) =>{

                if (i) {
                    let dp = {
                        api: 'miembros/vaciar',
                        id: this.dpi_formulario,
                        pull: false
                    }

                    await this.deleteData(dp)
                    this.set_datos_iglesia('')
                    this.set_dpi_formulario('')
                    this.set_nombre_formulario('')
                    await this.setSubModulo('InformacionGeneral')
                }
            })
        },
        ...mapActions(['deleteData']),
        ...mapMutations(['set_datos_iglesia', 'set_dpi_formulario', 'set_nombre_formulario'])
    },
}
</script>

<style>
    .titulo_formulario{
        text-align: center;
        font-size: 28px;
    }

    .btn_menu_formularios{
        width: 160px;
        height: 30px;
        position: absolute;
        top: 70px;
        right: 15px;
        display: flex;
    }
        .in_btn_menu_formularios{
            width: 30px;
            height: 30px;
            margin-right: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            user-select: none;
            border-radius: 50%;
            outline: none;
            border: none;
        }

        #btn_datos_personales{
            background-color: #1B998B;
            color: white;
            transition: .4s ease;
        }
            #btn_datos_personales:hover{
                background-color: #2D3047;
            }

        #btn_datos_iglesia{
            background-color: #175676;
            color: white;
            transition: .4s ease;
        }
            #btn_datos_iglesia:hover{
                background-color: #4BA3C3;
            }

        #btn_datos_hijos{
            background-color: #8A84E2;
            color: white;
            transition: .4s ease;
        }
            #btn_datos_hijos:hover{
                background-color: #84AFE6;
            }

        #btn_cancelar{
            background-color: #D62839;
            color: white;
            transition: .4s ease;
        }
            #btn_cancelar:hover{
                background-color: #FA8334;
            }
</style>