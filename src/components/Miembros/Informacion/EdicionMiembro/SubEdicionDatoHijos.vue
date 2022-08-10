<template>
    <div class="contenedor_modal">
        <div class="cuerpo_modal">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" size="sm" @click="cerrar">Cerrar</b-button>
            </div>
            <b-container fluid="">
                <form @submit.prevent="guardar">
                    <b-row>
                        <b-col sm="12" class="mt-3">
                            <label for="">Nombre</label>
                            <b-form-input type="text" v-model="nombre" size="sm"></b-form-input>
                        </b-col>
                        <b-col sm="12" class="mt-3">
                            <label for="">Fecha de nacimiento</label>
                            <b-form-input type="date" v-model="fecha_nacimiento" size="sm"></b-form-input>
                        </b-col>
                        <b-col sm="12" class="mt-3 d-flex flex-row-reverse">
                            <b-button type="submit" variant="primary" size="sm">Actualizar</b-button>
                        </b-col>
                    </b-row>
                </form>
            </b-container>
        </div>
    </div>
</template>

<script>
import moment from 'moment'
import { mapActions } from 'vuex'
export default {
    name: 'SubEdicionDatosHijos',
    props:['item'],
    data() {
        return {
            nombre: '',
            fecha_nacimiento: ''
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        },
        async guardar(){
            let f = {
                api: 'miembros/u/datos_hijos',
                id: this.item.id,
                pull: false,
                modo: 'param',
                formulario:{
                    nombre: this.nombre.toUpperCase().trim(),
                    fecha_de_nacimiento: this.fecha_nacimiento
                }
            }

            await this.updateData(f)
            await this.$emit('actualizar')
            this.cerrar()
        },
        ...mapActions(['updateData'])
    },
    mounted() {
        this.nombre = this.item.nombre,
        this.fecha_nacimiento = moment(this.item.fecha_de_nacimiento).format('YYYY-MM-DD')        
    },
}
</script>

<style>

</style>