<template>
    <div class="marcox">
        <b-container fluid="">
            <b-row v-if="subir_foto == false">
                <b-col sm="12">                
                    <div class="titulo_formulario">
                        Subir Fotos
                        <hr>
                    </div>
                </b-col>
                
                <b-col sm="12">                
                    <label for="">¿Se subirá foto ahora?</label>
                </b-col>
                <b-col sm="6">
                    <b-button type="button" variant="success" @click="consulta_subir_foto(true)" size="sm" block>Si</b-button>
                </b-col>
                <b-col sm="6">
                    <b-button type="button" variant="danger" @click="consulta_subir_foto(false)" size="sm" block>No</b-button>
                </b-col>
            </b-row>
            <b-row v-if="subir_foto == true">
                <!-- <b-col sm="12">                
                    <div class="titulo_formulario">
                        Subir Fotos
                        <hr>
                    </div>
                </b-col>
                <b-col sm="12" class="mt-3">
                    <input type="file" id="uploadfile2" @change="uploadFile" ref="btnUpload">
                    <b-button size="sm" variant="primary" block style="margin-left: 15px;" class="mb-2" @click="subirArchivo">
                        <b-icon icon="upload" aria-hidden="true"></b-icon> Subir Fotos
                    </b-button>
                </b-col> -->
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
                
                    <b-button size="sm" variant="warning" block v-if="imgSrc != ''"  @click="cropImage">
                        <b-icon icon="scissors" aria-hidden="true"></b-icon> Cortar y guardar
                    </b-button>
                </b-col>
                <b-col sm="3" class="mt-3">
                    <b-button size="sm" variant="info" block v-if="imgSrc != ''"  @click="rotate">
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
import { mapActions, mapMutations, mapState } from 'vuex'
import { atexto } from '../../functions/alertas'
import VueCropper from 'vue-cropperjs'

export default {
    name: 'SubirFotoFormulioMiembros',
    computed: {
        ...mapState(['dpi_formulario'])
    },
    components:{
        VueCropper
    },
    data() {
        return {
            subir_foto: false, // dejar en false
            imgSrc: "",
            cropImg: ""
        }
    },
    methods: {
        consulta_subir_foto(valor){
            if (valor) {
                this.subir_foto = true
            }else{
                this.set_dpi_formulario('')
                this.set_nombre_formulario('')
                atexto({titulo: 'Completo!', cuerpo: 'Se ha registrado correctamente', icono: 'success'})
                this.$emit('comprobacion', 'InformacionGeneral')
            }
        },
        subirArchivo(){
            this.$refs.btnUpload.click()
        },
        async uploadFile(event){
            let f = {
                api: 'fotos/archivos',
                evento: event.target.files[0],
                campos: [
                    {
                        campo: 'dpi',
                        payload: this.dpi_formulario
                    }
                ]
            }

            await this.sfile(f)
            this.set_dpi_formulario('')
            this.set_nombre_formulario('')

            atexto({titulo: 'Completo!', cuerpo: 'Se ha registrado correctamente', icono: 'success'})
            this.$emit('comprobacion', 'InformacionGeneral')
            //await this.obtenerDatos()
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
                    dpi: this.dpi_formulario,
                    photoData: this.cropImg
                }
            }
            
            await this.saveData(f)
            this.cropImg = ''
            this.set_dpi_formulario('')
            this.set_nombre_formulario('')

            atexto({titulo: 'Completo!', cuerpo: 'Se ha registrado correctamente', icono: 'success'})
            this.$emit('comprobacion', 'InformacionGeneral')
        },
        rotate() {
            // guess what this does :)
            this.$refs.cropper.rotate(90);
        },
        ...mapActions(['sfile', 'saveData']),
        ...mapMutations(['set_dpi_formulario', 'set_nombre_formulario'])
    },
}
</script>

<style>
    #uploadfile2{
        display: none;
    }

    .marcox{
        width: 100%;
        height: calc(100vh - 140px);
        padding-bottom: 10px;
        overflow: auto;
    }


    .area_de_corte{
        width: 500px;
        height: 550px;
        border: 1px solid gray;
        background-color: #e1e1e1;
        margin: auto;
    }

</style>