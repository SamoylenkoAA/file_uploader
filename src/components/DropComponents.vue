<template>
  <div id="file-drag-drop">
    <form ref="fileform">
      <span class="drop-files">Drop the files here!</span>
    </form>
    <div
      v-for="(file, key) in files"
      :key="key"
      class="file-listing"
    >
      <img class="preview" :ref="'preview'+parseInt(key)" />
      {{ file.name }}

      <div class="remove-container">
        <a class="remove" v-on:click="removeFile( key )">Remove</a>
      </div>
    </div>
    <button
      class="submit_button"
      v-show="files.length > 0"
      @click="submitFiles()"
    >
      Добавить
    </button>
  </div>
</template>

<script>
export default {
  name: "DropComponents",
  data(){
    return {
      dragAndDropCapable: false,
      files: [],
      files_base64: []
    }
  },
  methods: {
    determineDragAndDropCapable() {
      let div = document.createElement('div');
      return ( ( 'draggable' in div )
          || ( 'ondragstart' in div && 'ondrop' in div ) )
          && 'FormData' in window
          && 'FileReader' in window;
    },
    getImagePreviews() {
      for( let i = 0; i < this.files.length; i++ ){
        if ( /\.(jpe?g|png|gif)$/i.test( this.files[i].name ) ) {
          console.log(this.files[i])
          let reader = new FileReader();
          reader.addEventListener("load", () => {
            this.$refs['preview'+parseInt(i)].src = reader.result;
            this.files_base64.push(reader.result);
          }, false);
          reader.readAsDataURL( this.files[i] );
        }
      }
    },
    removeFile(key) {
      this.files.splice( key, 1 );
    },
    submitFiles() {
      this.files_base64.forEach(file => {
        console.log(file)
      })
    }
  },
  mounted(){
    this.dragAndDropCapable = this.determineDragAndDropCapable();
    if( this.dragAndDropCapable ){
      ['drag', 'dragstart', 'dragend', 'dragover', 'dragenter', 'dragleave', 'drop'].forEach(evt => {
        this.$refs.fileform.addEventListener(evt, (e) => {
          e.preventDefault();
          e.stopPropagation();
        }, false);
      });
      this.$refs.fileform.addEventListener('drop', e => {
        for( let i = 0; i < e.dataTransfer.files.length; i++ ){
          ( !this.files.find(file => file.name === e.dataTransfer.files[i].name ) ) ? this.files.push( e.dataTransfer.files[i] ) : null
          this.getImagePreviews();
        }
      });
    }
  }
}
</script>

<style scoped>
form {
  display: block;
  height: 150px;
  width: 400px;
  background: #ccc;
  margin: auto;
  text-align: center;
  line-height: 150px;
  border-radius: 4px;
}
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
.submit_button {
  display: block;
  margin: 20px auto;
  text-align: center;
  width: 200px;
  padding: 10px;
  text-transform: uppercase;
  background-color: #2c3e50;
  color: white;
  font-weight: bold;
  cursor: pointer;
}
</style>