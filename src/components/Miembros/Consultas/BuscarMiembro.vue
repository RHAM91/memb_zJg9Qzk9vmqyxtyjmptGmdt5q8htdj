<template>
    <b-container fluid="">
        <b-row>
            <b-col sm="12" class="mt-3">
                <h3>
                    Buscar
                </h3>
            </b-col>
            <b-col sm="2" class="mt-3">
                <select class="form-control form-control-sm" v-model="filtro">
                    <option value="">Filtro</option>
                    <option value="nombre">Nombre</option>
                    <option value="dpi">DPI</option>
                </select>
            </b-col>
            <b-col sm="10" class="mt-3">
                <b-input-group size="sm">
                    <b-input-group-prepend is-text>
                        <b-icon icon="search"></b-icon>
                    </b-input-group-prepend>
                    <b-form-input type="search" v-model="search" id="clie" size="sm" placeholder="Buscar" @keyup.enter="busquedaPorFiltro" autocomplete="off"></b-form-input>
                </b-input-group>
            </b-col>
            <b-col sm="12" class="mt-3">
                <b-table class="table-bordered table-striped" :items="registros" :fields="fields" :per-page="perPage" :current-page="currentPage" small style="font-size: 12px;">
                    
                    <template v-slot:cell(dpi) = 'idp'>
                        <div style="height: 40px;display: flex; justify-content:center;align-items:center;">
                            {{idp.item.dpi}}
                        </div>
					</template>

                    <template v-slot:cell(nombre) = 'nome'>
                        <div style="display: flex;align-items:center;height: 40px;">
                            {{nome.item.nombre}}
                        </div>
					</template>

                    <template v-slot:cell(direccion) = 'dire'>
                        <div style="display: flex;align-items:center;height: 40px;">
                            {{dire.item.direccion}}
                        </div>
					</template>

                    <template v-slot:cell(telefono_movil) = 'tel'>
                        <div style="display: flex;justify-content:center;align-items:center;height: 40px;">
                            {{tel.item.telefono_movil}}
                        </div>
					</template>
                    
                    <template v-slot:cell(btn) = 'row'>
                        <div style="display: flex; justify-content:center;align-items:center;height: 40px;">
                            <b-button type="button" size="sm" title="Ficha miembro" variant="warning" style="margin-right: 10px;" @click="abrir_modal_ficha(row.item.dpi, row.item.nombre)"><i class="fas fa-info-circle"></i></b-button>
                            <b-button v-if=" tipo == 'root' || permisos.miembros.actualizar == 1" type="button" size="sm" title="Editar ficha" variant="primary" @click="abrir_modal_edicion(row.item)"><i class="fas fa-pen"></i></b-button>
                        </div>
					</template>

				</b-table>
                <b-pagination
                    v-model="currentPage"
                    :total-rows="rows"
                    :per-page="perPage"
                    aria-controls="my-table"
                ></b-pagination>

            </b-col>
        </b-row>

        <FichaMiembro v-if="modalficha" :dpi="item_miembro" v-on:salir="cerrar_modal_ficha" />
        <EdicionMiembro v-if="modalEdicion" v-on:salir="cerrar_modal_edicion" />

    </b-container>
</template>

<script>
import { mapActions, mapMutations, mapState } from 'vuex'
import { minix } from '@/components/functions/alertas'

import FichaMiembro from './FichaMiembro.vue'
import EdicionMiembro from '../Informacion/EdicionMiembro/EdicionMiembro.vue'

export default {
    name: 'BuscarMiembro',
    components:{
        FichaMiembro,
        EdicionMiembro
    },
    computed: {
        rows(){
            return 5
        },
        ...mapState(['permisos', 'tipo'])
    },
    data() {
        return {
            search: '',
            modalficha: false,
            modalEdicion: false,
            item_miembro: '',
            filtro: 'nombre',

            registros: [],
            perPage: 15,
			currentPage: 1,
            fields: [
                {
                    key: 'dpi',
                    thStyle: 'width: 10%;text-align: center;',
                },
                {
                    key: 'nombre',
                    thStyle: 'width: 32%;',
                },
                {
                    key: 'direccion',
                    thStyle: 'width: 35%;'
                },
                {
                    key: 'telefono_movil',
                    thStyle: 'width: 8%;text-align:center;'
                },
                {
                    key: 'btn',
                    thStyle: 'width: 15%;text-align:center;'
                }
            ]

        }
    },
    methods: {
        async buscar(){
            // optimizar: se podría crear una ruta que solo devuelta el dpi, para que este se envie a la FichaMiembro.vue y alli si descargue toda la informacion segun el dpi
            if (this.search == '' || this.search == undefined) {
                minix({icon: 'info', mensaje: 'Escribe algo para buscar', tiempo: 3000})
                document.getElementById('clie').focus()
            }else{
                let data = {
                    api: `miembros/nombre/${this.search.toUpperCase()}`,
                }

                let r = await this.getData(data)
                this.registros = r
            }
        },
        async buscar_por_pdi(){
            if (this.search == '' || this.search == undefined) {
                minix({icon: 'info', mensaje: 'Escribe algo para buscar', tiempo: 3000})
                document.getElementById('clie').focus()
            }else{
                let data = {
                    api: `miembros/c/dpi/${this.search}`,
                }

                let r = await this.getData(data)
                this.registros = r
            }
        },
        async busquedaPorFiltro(){
            if (this.filtro == 'nombre') {
                this.buscar()
            }else if (this.filtro == 'dpi') {
                this.buscar_por_pdi()
            }
        },
        abrir_modal_ficha(item, nombre){
            this.modalficha = true
            this.item_miembro = item
        },
        cerrar_modal_ficha(){
            this.modalficha = false
            this.search = ''
            document.getElementById('clie').focus()
            
        },
        abrir_modal_edicion(item){
            this.set_datos_edicion_miembro(item)
            this.modalEdicion = true
        },
        cerrar_modal_edicion(){
            this.modalEdicion = false
            document.getElementById('clie').focus()
            this.buscar() // tentativo, esta funcion busca al momento de cerrar el modal, pero ver otras opciones para actualizar datos al cerrar ventana
        },
        ...mapActions(['getData']),
        ...mapMutations(['set_datos_edicion_miembro'])
    },
    mounted() {
        document.getElementById('clie').focus()
    },
}
</script>

<style>

</style>