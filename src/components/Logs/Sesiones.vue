<template>
    <b-container fluid="">
        <b-row>
            <b-col sm="12" class="mt-2">
                <h3>
                    Sesiones
                </h3>
            </b-col>
            <b-col sm="12" md="5" class="mt-3">
                <label for="">Fecha incial</label>
                <b-form-input type="date" v-model="inicio" size="sm"></b-form-input>
            </b-col>
            <b-col sm="12" md="5" class="mt-3">
                <label for="">Fecha final</label>
                <b-form-input type="date" v-model="final" size="sm"></b-form-input>
            </b-col>
            <b-col sm="12" md="2" class="mt-5">
                <b-button type="button" block variant="success" size="sm" @click="obtener_registros">Buscar</b-button>
            </b-col>

            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-striped table-bordered" style="font-size: 11px">
                    <thead>
                        <tr>
                            <th style="width: 10%;text-align: center;">
                                Usuario
                            </th>
                            <th style="width: 35%;">
                                Nombre
                            </th>
                            <th style="width: 15%;text-align: center;">
                                Ubicación
                            </th>
                            <th style="width: 15%;text-align: center;">
                                Fecha
                            </th>
                            <th style="width: 15%;text-align: center;">
                                Hora (24hrs)
                            </th>
                            <th style="width: 10%;text-align: center;">
                                ...
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in datos" :key="index">
                            <td style="text-align: center;">
                                {{item.idUsuario}}
                            </td>
                            <td>
                                {{item.nombre}}
                            </td>
                            <td style="text-align: center;">
                                {{obtener_ciudad(item.ubicacion)}}
                            </td>
                            <td style="text-align: center;">
                                {{item.creacion | nfecha}}
                            </td>
                            <td style="text-align: center;">
                                {{item.hora}}
                            </td>
                            <td style="text-align: center;">
                                <b-button type="button" variant="outline-info" style="font-size: 9px;" @click="abrir_mapa(item.ubicacion)" size="sm">U</b-button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
import moment from 'moment'
import { mapActions } from 'vuex'

export default {
    name: 'Sesiones',
    filters: {
        nfecha: function(val){
            return moment(val).format('DD-MM-YYYY')
        }
    },
    data() {
        return {
            inicio: moment(Date.now()).format('YYYY-MM-DD'),
            final: moment(Date.now()).format('YYYY-MM-DD'),
            datos: []
        }
    },
    methods: {
        async obtener_registros(){
            let i = {
                api: `logs/sesiones/?inicio=${this.inicio}&final=${this.final}`,
            }

            let r = await this.getData(i)
            this.datos = r

        },
        obtener_ciudad(data){
            if (data == null || data == '') {
                return 'SIN DATOS'
            }else{
                let raw = JSON.parse(data)
                return raw.city
            }
        },
        abrir_mapa(datos){

            try {
                let coordenadas = JSON.parse(datos).ll
                window.open(`https://www.google.com/maps/search/${coordenadas[0]},+${coordenadas[1]}/@${coordenadas[0]},${coordenadas[1]},17z`)
            } catch (e) {
                alert('No hay datos')
            }

        },
        ...mapActions(['getData'])
    },
}
</script>

<style>

</style>