<template>
    <div class="contenedor_modal">
        <div class="cuerpo_modal">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" size="sm" @click="cerrar">Cerrar</b-button>
            </div>
            <b-container fluid="">
                <b-row>
                    <b-col sm="3" class="mt-3">
                        <label for="">Parentezco</label>
                        <select class="form-control form-control-sm" id="parentezco"  v-model="parentezco">
                            <option value="">Selecciona</option>
                            <option value="PADRE">Padre</option>
                            <option value="MADRE">Madre</option>
                            <option value="HIJO">Hijo</option>
                            <option value="HIJA">Hija</option>
                            <option value="HERMANO">Hermano</option>
                            <option value="HERMANA">Hermana</option>
                            <option value="PRIMO">Primo</option>
                            <option value="PRIMA">Tío</option>
                            <option value="TÍO">Tía</option>
                            <option value="TÍA">Tía</option>
                            <option value="ABUELO">Abuelo</option>
                            <option value="ABUELA">Abuela</option>
                        </select>
                    </b-col>
                    <b-col sm="7" class="mt-3">
                        <label for="">Nombre del familiar</label>
                        <b-form-input type="text" id="nombre_familiar" v-model="nombre_familiar" placeholder="Separar por comas"  size="sm"></b-form-input>
                    </b-col>

                    <b-col sm="2" class="mt-5">
                        <b-button type="button" block variant="outline-primary" size="sm" @click="actualizar_fila">Actualizar</b-button>
                    </b-col>
                </b-row>
            </b-container>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ModFamiliares',
    props:['index_row', 'item_row'],
    data() {
        return {
            parentezco: '',
            nombre_familiar: ''
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        },
        actualizar_fila(){
            let f = {
                index: this.index_row,
                datos: {
                    parentezco: this.parentezco.toUpperCase().trim(),
                    nombre: this.nombre_familiar.toUpperCase().trim(),
                }
            }

            this.$emit('modificacion_parentezco', f)
            this.cerrar()
        }
    },
    mounted() {
        this.parentezco = this.item_row.parentezco
        this.nombre_familiar = this.item_row.nombre
    },
}
</script>

<style>

</style>