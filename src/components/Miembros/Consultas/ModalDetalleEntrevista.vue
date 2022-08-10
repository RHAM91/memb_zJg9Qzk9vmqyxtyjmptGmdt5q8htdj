<template>
    <div class="contenedor_modal">
        <div class="cuerpo_modal mod_modal">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" @click="cerrar" size="sm">Cerrar</b-button>
            </div>
            <b-container fluid>
                <b-row>
                    <b-col sm="12" class="mt-3">
                        <div class="titulo_nombre">
                            {{ient.nombre}}
                        </div>
                    </b-col>
                    <b-col sm="12" class="mt-1">
                        DPI: {{ient.dpi}}
                    </b-col>
                    <b-col sm="12" class="mt-2">
                        <hr>
                    </b-col>
                    <b-col sm="12" class="mt-1">
                        <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                            <thead>
                                <tr>
                                    <th style="width: 25%;">
                                        .
                                    </th>
                                    <th style="width: 75%;">
                                        ..
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <td>
                                        Pregunta 1
                                    </td>
                                    <td>
                                        {{resultados.pregunta_1}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 2
                                    </td>
                                    <td>
                                        {{resultados.pregunta_2}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 3
                                    </td>
                                    <td>
                                        {{resultados.pregunta_3}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 4
                                    </td>
                                    <td>
                                        {{resultados.pregunta_4}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 5
                                    </td>
                                    <td>
                                        {{resultados.pregunta_5}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 6
                                    </td>
                                    <td>
                                        {{resultados.pregunta_6}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 7
                                    </td>
                                    <td>
                                        {{resultados.pregunta_7}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 8
                                    </td>
                                    <td>
                                        {{resultados.pregunta_8}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 9
                                    </td>
                                    <td>
                                        {{resultados.pregunta_9}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Pregunta 10
                                    </td>
                                    <td>
                                        {{resultados.pregunta_10}}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Comentario
                                    </td>
                                    <td>
                                        {{resultados.comentarios}}
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </b-col>
                    <b-col v-if=" tipo == 'root' || permisos.aprobacion_miembros.actualizar == 1" sm="6" class="mt-3">
                        <b-button type="button" variant="danger" size="sm" @click="descartar">Descartar</b-button>
                    </b-col>
                    <b-col v-if=" tipo == 'root' || permisos.aprobacion_miembros.actualizar == 1" sm="6" class="mt-3 d-flex flex-row-reverse">
                        <b-button type="button" variant="success" size="sm" @click="aprobar">Aprobar</b-button>
                    </b-col>
                </b-row>
            </b-container>
        </div>
    </div>
</template>

<script>
import { mapActions, mapState } from 'vuex'
import { pregunta } from '../../functions/alertas'
export default {
    name: 'ModalDetalleEntrevistas',
    props: ['ient'],
    computed: {
        ...mapState(['permisos', 'tipo'])
    },
    data() {
        return {
            resultados: {}
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        },
        async obtenerDatos(){
            let r = await this.getData({api: `entrevistas/${this.ient.dpi}`})
            this.resultados = r
        },
        async descartar(){

            pregunta({titulo: 'Seguro que deseas descartarlo?', texto: 'Esta acción no se puede revertir', afirmacion: 'Si, descartarlo!'}, async (i) =>{

                if (i) {
                    let f = {
                        api: 'aprobar_miembros',
                        id: this.ient.dpi,
                        modo: 'param',
                        pull: true,
                        formulario: {
                            asignado: 2
                        }
                    }

                    await this.updateData(f)
                    await this.cerrar()
                }
            })
            
        },
        async aprobar(){

            pregunta({titulo: 'Seguro que deseas aprobarlo?', texto: 'A partir de este momento la persona podrá ser tomanda en cuenta como miembro', afirmacion: 'Si, aprobarlo!'}, async (i) =>{

                if (i) {
                    let f = {
                        api: 'aprobar_miembros',
                        id: this.ient.dpi,
                        modo: 'param',
                        pull: true,
                        formulario: {
                            asignado: 4
                        }
                    }

                    await this.updateData(f)
                    await this.cerrar()
                }
            })
            
        },
        ...mapActions(['getData', 'updateData'])
    },
    mounted() {
        this.obtenerDatos()
    },
}
</script>

<style>
    .mod_modal{
        width: 800px !important;
    }

    .titulo_nombre{
        font-size: 25px;
    }
</style>