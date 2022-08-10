<template>
    <div class="contenedor_modal">
        <div class="cuerpo_modal">
            <div class="cabecera_modal">
                <b-button type="button" variant="outline-danger" size="sm" @click="cerrar">Cerrar</b-button>
            </div>
            <b-container fluid="">
                <b-row>
                    <b-col sm="12" class="mt-3">
                        <h2>
                            Familiares
                        </h2>
                    </b-col>
                    <b-col sm="12" class="mt-3">
                        <table class="table table-sm table-striped table-bordered" style="font-size: 11px;">
                            <thead>
                                <tr>
                                    <th style="width: 20%;">
                                        Parentezco
                                    </th>
                                    <th style="width: 80%;">
                                        Nombre
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in familiares" :key="index">
                                    <td>
                                        {{item.parentezco}}
                                    </td>
                                    <td>
                                        {{item.nombre}}
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </b-col>
                </b-row>
            </b-container>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ModalVerFamiliares',
    props: ['familiares'],
    data() {
        return {
            
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        }
    },
}
</script>

<style>

</style>