<template>
    <b-container fluid="">
        <!-- <form @submit.prevent="consultar"> -->
            <b-row>
                <b-col sm="12" class="mt-3">
                    <h2>
                        Reporte de miembros
                    </h2>
                </b-col>
                <b-col sm="10" class="mt-3">
                    <label for="">Tipo</label>
                    <select  class="form-control form-control-sm" @change="reste_select" required v-model="tipo_reporte">
                        <optgroup label="---------- GENERAL -----------">
                            <option value="">Selecciona</option>
                            <option value="general">General</option>
                            <option value="servidores">Servidores</option>
                            <option value="servidores_mostrar_areas">Servidores (mostrar áreas)</option>
                            <option value="no_servidores">Sin Privilegio</option>
                        </optgroup>
                        <optgroup label="---------- POR FECHA -----------">
                            <option value="generalxfecha">General</option>
                            <option value="servidoresxfecha">Servidores</option>
                        </optgroup>
                        <!-- <option value="xfecha">Por Fecha</option> -->
                    </select>
                </b-col>

                <b-col v-if="tipo_reporte == 'general' || tipo_reporte == 'servidores' || tipo_reporte == 'no_servidores' || tipo_reporte == 'servidores_mostrar_areas'" sm="2" class="mt-5">
                    <b-button type="button" block variant="info" size="sm" @click="consultar">Consultar</b-button>
                </b-col>


                <b-col v-if="tipo_reporte == 'generalxfecha' || tipo_reporte == 'servidoresxfecha'" sm="5" class="mt-3">
                    <label for="">Fecha inicial</label>
                    <b-form-input type="date" v-model="fecha_inicial" size="sm"></b-form-input>
                </b-col>
                <b-col v-if="tipo_reporte == 'generalxfecha' || tipo_reporte == 'servidoresxfecha'" sm="5" class="mt-3">
                    <label for="">Fecha final</label>
                    <b-form-input type="date" v-model="fecha_final" size="sm"></b-form-input>
                </b-col>

                <b-col v-if="tipo_reporte == 'generalxfecha' || tipo_reporte == 'servidoresxfecha'" sm="2" class="mt-5">
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
                                    <th style="width: 25%;">
                                        Nombre
                                    </th>
                                    <th style="width: 15%;text-align: center;">
                                        Teléfono casa
                                    </th>
                                    <th style="width: 15%;text-align: center;">
                                        Teléfono móvil
                                    </th>
                                    <th v-if="tipo_reporte == 'servidores_mostrar_areas'" style="width: 10%;text-align: center;">
                                        Áreas
                                    </th>
                                    <th style="width: 30%;text-align: center;">
                                        Dirección
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in resultados" :key="index">
                                    <td style="text-align: center;">
                                        {{index + 1}}
                                    </td>
                                    <td>
                                        {{item.nombre}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.telefono}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.telefono_movil}}
                                    </td>
                                    <td v-if="tipo_reporte == 'servidores_mostrar_areas'" style="text-align: center;">
                                        {{formato_areas(item.areas)}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.direccion}}
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
    name: 'ReportePorMiembros',
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
            resultados: [],
            tipo_reporte: 'general',
            fecha_inicial: moment(Date.now()).format('YYYY-MM-DD'),
            fecha_final: moment(Date.now()).format('YYYY-MM-DD')
        }
    },
    methods: {
        async consultar(){

            if (this.tipo_reporte == '' || this.tipo_reporte == undefined) {
                minix({icon: 'info', mensaje: 'Debes seleccionar un tipo de reporte', tiempo: 3000})
            }else{
                if (this.tipo_reporte == 'general') {
                    let f = {
                        api: `miembros`,
                    }
        
                    let r = await this.getData(f)
                    this.resultados = r

                }else if (this.tipo_reporte == 'xfecha') {

                    if (this.fecha_inicial == '' || this.fecha_final == '') {
                        minix({icon: 'info', mensaje: 'Debes seleccionar rango de fechas', tiempo: 3000})
                    }else{
                    
                        let f = {
                            api: `miembros/c/miembros_por_area_rango_fecha/?area=${this.area}&fecha_inicial=${this.fecha_inicial}&fecha_final=${this.fecha_final}`,
                        }
            
                        let r = await this.getData(f)
                        this.resultados = r

                    }
                }else if(this.tipo_reporte == 'servidores'){

                    let f = {
                        api: `asignaciones/r/miembros_web/1`,
                    }

                    let r = await this.getData(f)
                    this.resultados = r

                }else if(this.tipo_reporte == 'no_servidores'){
                    let f = {
                        api: `asignaciones/r/miembros_web/2`,
                    }

                    let r = await this.getData(f)
                    this.resultados = r

                }else if(this.tipo_reporte == 'servidores_mostrar_areas'){
                    let f = {
                        api: 'asignaciones/r/miembros_web/3'
                    }

                    let r = await this.getData(f)
                    this.resultados = r
                }else if(this.tipo_reporte == 'generalxfecha'){

                    if (this.fecha_inicial == '' || this.fecha_final == '') {
                        minix({icon: 'info', mensaje: 'Debes seleccionar rango de fechas', tiempo: 3000})

                    }else{
                        let f = {
                            api: `miembros/r/miembros_general_por_fecha/?fecha_inicial=${this.fecha_inicial}&fecha_final=${this.fecha_final}`
                        }
                        

                        let r = await this.getData(f)
                        this.resultados = r
                    }

                }else if(this.tipo_reporte == 'servidoresxfecha'){

                    if (this.fecha_inicial == '' || this.fecha_final == '') {
                        minix({icon: 'info', mensaje: 'Debes seleccionar rango de fechas', tiempo: 3000})

                    }else{
                        let f = {
                            api: `miembros/r/miembros_servidores_por_fecha/?fecha_inicial=${this.fecha_inicial}&fecha_final=${this.fecha_final}`
                        }
                        

                        let r = await this.getData(f)
                        this.resultados = r
                    }

                }
            }
        },
        reste_select(){
            this.resultados = []
        },
        formato_areas(arr){
            
            let narr = []

            for (let i = 0; i < arr.length; i++) {
                const e = arr[i];
                
                let mf = this.areas.filter(z => z.id == e.area)
                narr.push(mf[0].area)

            }


            let marr = narr.join('\n')

            return marr
            
        },
        dns_privilegios(id){
            let r = this.privilegios.filter(z => z.id == id)
            return r[0].privilegio
        },
        async generar_pdf(){

            if (this.tipo_reporte == '' || this.tipo_reporte == undefined) {
                minix({icon: 'info', mensaje: 'Debes seleccinar un área', tiempo: 3000})
            }else{

                if (this.tipo_reporte == 'general') {
                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_miembros_general.php?token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)

                }else if(this.tipo_reporte == 'xfecha'){
                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_por_area_rango_fecha.php?area=${this.area}&token=${this.$store.state.token.headers.Authorization.split(' ')[1]}&fecha_inicial=${this.fecha_inicial}&fecha_final=${this.fecha_final}`)
                }else if(this.tipo_reporte == 'servidores'){
                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_miembros_servidores.php?token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)
                }else if(this.tipo_reporte == 'servidores_mostrar_areas'){
                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_miembros_servidores_mostrar_areas.php?token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)
                }else if(this.tipo_reporte == 'no_servidores'){
                    window.open(`http://${IP}/reportes/membresias/ficha/reporte_miembros_sin_privilegios.php?token=${this.$store.state.token.headers.Authorization.split(' ')[1]}`)
                }

            }
        },
        async generar_excel(){
            if (this.tipo_reporte == '' || this.tipo_reporte == undefined) {
                minix({icon: 'info', mensaje: 'Debes seleccinar un área', tiempo: 3000})
            }else{

                
                if (this.tipo_reporte == 'general') {
                    let f = {
                        api: 'miembros/r/miembros_general_excel',
                    }
                    let r = await this.getData(f)

                    if (r.message == 'Descargando...') {
                        minix({icon: 'info', mensaje: r.message, tiempo: 3000})
                        window.open(`http://${IP}:${PUERTO}/xlsx/reporte_general_miembros.xlsx`)
                    }
                }else if(this.tipo_reporte == 'servidores'){
                    let f = {
                        api: 'asignaciones/r/servidores_a_excel/1', // el 1 es para el reporte sin areas
                    }
                    let r = await this.getData(f)

                    if (r.message == 'Descargando...') {
                        minix({icon: 'info', mensaje: r.message, tiempo: 3000})
                        window.open(`http://${IP}:${PUERTO}/xlsx/reporte_general_servidores.xlsx`)
                    }
                }else if(this.tipo_reporte == 'servidores_mostrar_areas'){
                    let f = {
                        api: 'asignaciones/r/servidores_a_excel/2', // el 1 es para el reporte sin areas
                    }
                    let r = await this.getData(f)

                    if (r.message == 'Descargando...') {
                        minix({icon: 'info', mensaje: r.message, tiempo: 3000})
                        window.open(`http://${IP}:${PUERTO}/xlsx/reporte_general_servidores.xlsx`)
                    }
                }else if(this.tipo_reporte == 'no_servidores'){
                    let f = {
                        api: 'asignaciones/r/no_servidores_a_excel',
                    }
                    let r = await this.getData(f)

                    if (r.message == 'Descargando...') {
                        minix({icon: 'info', mensaje: r.message, tiempo: 3000})
                        window.open(`http://${IP}:${PUERTO}/xlsx/reporte_general_no_servidores.xlsx`)
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

        .marcox__{
            width: 100%;
            height: calc(100vh - 230px);
            overflow: auto;

        }

        .marcox___{
            width: 100%;
            height: calc(100vh - 320px);
            overflow: auto;
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