<template>
  <div
      v-for="(file, key) in files"
      :key="key"
      class="file-listing"
  >
    <img class="preview" :ref="'preview'+parseInt(key)" />
    {{ file.name }}

    <div class="remove-container">
      <a class="remove" v-on:click="$emit('removeFile', key)">Remove</a>
    </div>
  </div>
</template>

<script>
export default {
  name: "PreviewImages",
  props: {
    files: {
      type: Array,
      default: () => {}
    }
  },
  emits: ['removeFile'],
  methods: {
    getImagePreviews() {
      this.files.forEach((file, key) => {
        if ( /\.(jpe?g|png|gif)$/i.test( file.name ) ) {
          let reader = new FileReader();
          reader.readAsDataURL(file);

          reader.onload = () => {
            this.$refs['preview' + parseInt(key)].src = reader.result;
          };
        }
      })
    },
  }
}
<img src="../assets/logo.png" height="200" width="200"/></script>

<style scoped>
div.file-listing{
  width: 400px;
  margin: auto;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}
div.file-listing img{
  height: 100px;
}
div.remove-container{
  text-align: center;
  margin: 10px;
  border: 1px solid red;
}
div.remove-container a{
  color: red;
  cursor: pointer;
}
</style>