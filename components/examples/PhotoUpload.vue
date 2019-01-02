<template>
    <div>
        <h1> Choose your avatar !</h1>
        <div style="background-color: #7f828b">
            <img ref="picPreview" :src="pictureInstance.value" height="200" alt="Image preview...">
        </div>
        <br><br>
        <input ref="picInput" type="file" v-on:change="previewPicture()">
    </div>
</template>

<script>
    export default {
        name: "PhotoUpload",
        props: {
          pictureInstance: {
              type: Object,
              default: {value: ""}
          }
        },
        methods: {
            previewPicture() {
                const file = this.$refs.picInput.files[0];
                const reader = new FileReader();

                file ? reader.readAsDataURL(file) : this.pictureInstance.value = "";
                reader.onloadend = () => this.pictureInstance.value = reader.result;
            }
        }
    }
</script>

<style scoped>

</style>
