<template>
    <div class="contenedor_modal">
        <div class="cuerpo_modal">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" @click="salir" size="sm">Cerrar</b-button>
            </div>
            <b-container fluid="">
                <form @submit.prevent="actualizar">
                    <b-row>
                        <b-col sm="10" class="mt-3">
                            <label for="">Nombre de la profesión</label>
                            <b-form-input type="text" id="campoprofesion" v-model="campoprofesion" autocomplete="off" required size="sm"></b-form-input>
                        </b-col>
                        <b-col sm="2" class="mt-3">
                            <b-button type="submit" size="sm" variant="primary" block style="margin-top: 31px;">Actualizar</b-button>
                        </b-col>
                    </b-row>
                </form>
            </b-container>
        </div>
    </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
    name: 'ActualizarProfesion',
    props: ['itemProfesion'],
    data() {
        return {
            campoprofesion: this.itemProfesion.profesion
        }
    },
    methods: {
        salir(){
            this.$emit('salir')
        },
        async actualizar(){
            let data = {
                api: 'profesiones',
                id: this.itemProfesion.id,
                modo: 'param',
                pull: true,
                formulario: {
                    profesion: this.campoprofesion.toUpperCase()
                }
            }

            await this.updateData(data)
            await this.salir()
        },
        ...mapActions(['updateData'])
    },
    mounted() {
        document.getElementById('campoprofesion').focus()
    },
}
</script>

<style>

</style>