<template>
    <div class="contenedor_modal mod_foto_grande">
        <div class="foto_grande">
            <img :src="iMiembro" class="img_foto_grande" alt="">
        </div>
        <div class="btn_cerrar_foto_grande">
            <b-button  type="button" variant="outline-danger" @click="cerrar" size="sm">Cerrar</b-button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ContenedorModal',
    props:['iMiembro'],
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
    .foto_grande{
        width: 500px;
        height: 625px;
        background-color: white;
        
    }
        .img_foto_grande{
            width: 100%;
            object-fit: cover;
        }

        .mod_foto_grande{
            background-color: rgba(255, 255, 255, 0.9) !important;
            
        }
            .btn_cerrar_foto_grande{
                position: fixed;
                top: 15px;
                right: 15px;

            }
</style>