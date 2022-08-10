<template>
    <b-container fluid="">
        <!-- <form @submit.prevent="consultar"> -->
            <b-row>
                <b-col sm="12" class="mt-3">
                    <h2>
                        Reporte por área
                    </h2>
                </b-col>
                <b-col sm="4" class="mt-3">
                    <label for="">Tipo</label>
                    <select  class="form-control form-control-sm" @change="reste_select" required v-model="tipo_reporte">
                        <option value="">Selecciona</option>
                        <option value="general">General</option>
                        <option value="general_foto">General con foto</option>
                        <option value="xfecha">Por Fecha</option>
                    </select>
                </b-col>



                <b-col v-if="tipo_reporte == 'xfecha'" sm="4" class="mt-3">
                    <label for="">Fecha inicial</label>
                    <b-form-input type="date" v-model="fecha_inicial" size="sm"></b-form-input>
                </b-col>
                <b-col v-if="tipo_reporte == 'xfecha'" sm="4" class="mt-3">
                    <label for="">Fecha final</label>
                    <b-form-input type="date" v-model="fecha_final" size="sm"></b-form-input>
                </b-col>



                <b-col v-if="tipo_reporte == 'general' || tipo_reporte == 'general_foto'" sm="8" class="mt-3">
                    <label for="">Área</label>
                    <select  class="form-control form-control-sm" required v-model="area" @change="consultar">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in areas" :key="index" :value="item.id">{{item.area}}</option>
                    </select>
                </b-col>

                <b-col v-if="tipo_reporte == 'xfecha'" sm="10" class="mt-3">
                    <label for="">Área</label>
                    <select  class="form-control form-control-sm" required v-model="area">
                        <option value="">Selecciona</option>
                        <option v-for="(item, index) in areas" :key="index" :value="item.id">{{item.area}}</option>
                    </select>
                </b-col>
                <b-col v-if="tipo_reporte == 'xfecha'" sm="2" class="mt-5">
                    <b-button type="button" block variant="info" size="sm" @click="consultar">Consultar</b-button>
                </b-col>

                <b-col sm="12" class="mt-3">
                    <div :class="[tipo_reporte == 'general' ? 'marcox__' : 'marcox___']">
                        <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                            <thead>
                                <tr>
                                    <th style="width: 5%;text-align: center;">
                                        No.
                                    </th>
                                    <th style="width: 40%;">
                                        Nombre
                                    </th>
                                    <th style="width: 15%;text-align: center;">
                                        Teléfono móvil
                                    </th>
                                    <th style="width: 15%;text-align: center;">
                                        Privilegio
                                    </th>
                                    <th style="width: 25%;text-align: center;">
                                        Comentarios
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in resultados" :key="index">
                                    <td style="text-align: center;">
                                        {{item.dpi}}
                                    </td>
                                    <td>
                                        {{item.miembro}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.telefono}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{dns_privilegios(item.privilegio)}}
                                    </td>
                                    <td style="text-align: center;">
                                        
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </b-col>
            </b-row>
        <!-- </form> -->

        <div v-if="resultados.length != 0" title="Exportar a Excel" class="btn_excel" @click="generar_excel">
            <i class="fas fa-file-excel"></i>
        </div>
        <div v-if="resultados.length != 0" title="Exportar a PDF" class="btn_pdf" @click="generar_pdf">
            <i class="fas fa-file-pdf"></i>
        </div>

    </b-container>
</template>

<script>
import { IP, PUERTO } from '@/config/parametros'
import { mapActions, mapState } from 'vuex'
import { minix } from '../functions/alertas'
import moment from 'moment'
export default {
    name: 'ReportePorArea',
    computed: {
        ...mapState(['areas', 'privilegios'])
    },
    filters:{
        nfecha: function(val){
            return moment(val).format('DD-MM-YYYY')
        }
    },
    data() {
        return {
            area: '',
            resultados: [],
            tipo_reporte: 'general',
            fecha_inicial: moment(Date.now()).format('YYYY-MM-DD'),
            fecha_final: moment(Date.now()).format('YYYY-MM-DD')
        }
    },
    methods: {
        async consultar(){

            if (this.area == '' || this.area == undefined) {
                minix({icon: 'info', mensaje: 'Debes seleccionar un área', tiempo: 3000})
            }else{

                if (this.fecha_inicial == '' || this.fecha_final == '') {
                    minix({icon: 'info', mensaje: 'Debes seleccionar rango de fechas', tiempo: 3000})
                }else{

                    if (this.tipo_reporte == 'general' || this.tipo_reporte == 'general_foto') {
                        let f = {
                            api: `miembros/c/miembros_por_area/${this.area}`,
                        }
            
                        let r = await this.getData(f)
                        this.resultados = r
                    }else if (this.tipo_reporte == 'xfecha') {
                        
                        let f = {
                            api: `miembros/c/miembros_por_area_rango_fecha/?area=${this.area}&fecha_inicial=${this.fecha_inicial}&fecha_final=${this.fecha_final}`,
                        }
            
                        let r = await this.getData(f)
                        this.resultados = r

                    }

                }
            }

        },
        dns_area(id){
            let r = this.areas.filter(z => z.id == id)
            return r[0].area
        },
        dns_privilegios(id){
            let r = this.privilegios.filter(z => z.id == id)
            return r[0].privilegio
        },
        reste_select(){
            this.area = ''
            this.resultados = []
        },
        async generar_pdf(){

            if (this.area == '' || this.area == undefined) {
                minix({icon: 'info', mensaje: 'Debes seleccinar un área', tiempo: 3000})
            }else{

                if (this.tipo_reporte == 'general') {
                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_general_por_area.php?area=${this.area}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)

                }else if(this.tipo_reporte == 'general_foto'){

                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_miembros_con_foto.php?area=${this.area}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)

                }else if(this.tipo_reporte == 'xfecha'){
                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_por_area_rango_fecha.php?area=${this.area}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}&fecha_inicial=${this.fecha_inicial}&fecha_final=${this.fecha_final}`)
                }

            }

        },
        async generar_excel(){
            if (this.area == '' || this.area == undefined) {
                minix({icon: 'info', mensaje: 'Debes seleccinar un área', tiempo: 3000})
            }else{

                if (this.area == '' || this.area == undefined) {
                    minix({icon: 'info', mensaje: 'Debes seleccinar un área', tiempo: 3000})
                }else{
                    if (this.tipo_reporte == 'general') {
                        
                        let f = {
                            api: `miembros/r/miembros_areas_general_excel/${this.area}`,
                        }
                        let r = await this.getData(f)
    
                        if (r.message == 'Descargando...') {
                            minix({icon: 'info', mensaje: r.message, tiempo: 3000})
                            window.open(`http://${IP}:${PUERTO}/xlsx/reporte_general_por_areas.xlsx`)
                        }
    
                    }else if(this.tipo_reporte == 'xfecha'){
                        let f = {
                            api: `miembros/r/miembros_por_area_rango_fecha_excel/?area=${this.area}&fecha_inicial=${this.fecha_inicial}&fecha_final=${this.fecha_final}`
                        }

                        let r = await this.getData(f)

                        if(r.message == 'Descargando...'){
                            minix({icon: 'info', mensaje: r.message, tiempo: 3000})
                            window.open(`http://${IP}:${PUERTO}/xlsx/reporte_general_por_areas_fecha_excel.xlsx`)

                        }

                    }else{
                        minix({icon: 'info', mensaje: 'NO DISPONIBLE', tiempo: 3000})
                    }

                }

            }
        },
        ...mapActions(['getData'])
    },
}
</script>

<style>
    .btn_pdf{
        position: fixed;
        bottom: 15px;
        right: 15px;
        width: 45px;
        height: 45px;
        border-radius: 50%;
        background-color: #DD1C1A;
        cursor: pointer;
        user-select: none;
        display: flex;
        justify-content: center;
        align-items: center;
        transition: .4s ease;
        color: white;
    }
        .btn_pdf:hover{
            background-color: #F56E65;
        }

    .btn_excel{
        position: fixed;
        bottom: 70px;
        right: 15px;
        width: 45px;
        height: 45px;
        border-radius: 50%;
        background-color: #037b23;
        cursor: pointer;
        user-select: none;
        display: flex;
        justify-content: center;
        align-items: center;
        transition: .4s ease;
        color: white;
    }
        .btn_excel:hover{
            background-color: #F56E65;
        }
</style>