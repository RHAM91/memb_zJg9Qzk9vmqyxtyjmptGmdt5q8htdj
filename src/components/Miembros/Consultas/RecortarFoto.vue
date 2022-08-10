<template>
    <div class="contenedor_full">
        <div class="banner_full_">
            <b-button type="button" variant="outline-danger" size="sm" @click="cerrar">Cerrar</b-button>
        </div>
        <b-container fluid="">
            <b-row>
                <b-col sm="12" class="mt-3">
                    <label for="">Elegir foto</label>
                    <b-form-file accept="image/*" size="sm" @change="setImage"></b-form-file>
                </b-col>
                <b-col sm="12" class="mt-3">
                    <div class="area_de_corte">
                        <VueCropper

                            ref="cropper"
                            :guides="true"
                            :viewMode="0"
                            drag-mode="move"
                            :cropBoxMovable="false"
                            :cropBoxResizable="false"
                            :auto-crop-area="0.4"
                            :minCropBoxWidth="200"
                            :minCropBoxHeight="250"
                            :background="true"
                            :rotatable="true"
                            :src="imgSrc"
                            alt="Fuente"
                            :img-style="{ width: '500px', height: '550px' }"                
                        />
                    </div>
                </b-col>
                <b-col sm="3" class="mt-3">
                            
                </b-col>
                <b-col sm="3" class="mt-3">
                
                    <b-button size="sm" variant="warning" block v-if="imgSrc != ''" class="mb-2" @click="cropImage">
                        <b-icon icon="scissors" aria-hidden="true"></b-icon> Cortar y guardar
                    </b-button>
                </b-col>
                <b-col sm="3" class="mt-3">
                    <b-button size="sm" variant="info" block v-if="imgSrc != ''" class="mb-2" @click="rotate">
                        <b-icon icon="arrow-counterclockwise" aria-hidden="true"></b-icon> Girar
                    </b-button>
                </b-col>
                <b-col sm="3" class="mt-3">
                    
                </b-col>

            </b-row>
        </b-container>
    </div>
</template>

<script>

import VueCropper from 'vue-cropperjs'
import { mapActions, mapState } from 'vuex'

export default {
    name: 'RecortarFoto',
    components:{
        VueCropper
    },
    props:['iMiembro'],
    data() {
        return {
            imgSrc: "",
            cropImg: ""
        }
    },
    methods: {
        cerrar(){
            this.$emit('salir')
        },
        setImage(e) {
            const file = e.target.files[0];

            if (!file.type.includes("image/")) {
                alert("Please select an image file");
                return;
            }

            if (typeof FileReader === "function") {
                const reader = new FileReader();

                reader.onload = event => {
                this.imgSrc = event.target.result;
                // rebuild cropperjs with the updated source
                this.$refs.cropper.replace(event.target.result);
                };

                reader.readAsDataURL(file);
            } else {
                alert("Sorry, FileReader API not supported");
            }
        },
        async cropImage() {
            // get image data for post processing, e.g. upload or setting image src
            this.cropImg = this.$refs.cropper.getCroppedCanvas().toDataURL('image/png').replace(/^data:image\/(png|jpg|jpeg);base64,/, '')

            let f = {
                api: 'fotos',
                pull: false,
                formulario:{
                    dpi: this.iMiembro.dpi,
                    photoData: this.cropImg
                }
            }
            
            await this.saveData(f)
            this.cropImg = ''

            this.cerrar()
        },
        rotate() {
            // guess what this does :)
            this.$refs.cropper.rotate(90);
        },
        ...mapActions(['saveData'])
    },
}
</script>

<style>

</style>