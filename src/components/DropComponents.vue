<template>
  <div id="file-drag-drop">
    <form ref="fileform">
      <span class="drop-files">Drop the files here!</span>
    </form>
    <preview-images
      ref="previewImg"
      :files="files"
      @removeFile="removeFile($event)"
    />
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
import PreviewImages from '@/components/PreviewImages';

export default {
  name: "DropComponents",
  components: { PreviewImages },
  data(){
    return {
      dragAndDropCapable: false,
      files: []
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
    removeFile(key) {
      this.files.splice( key, 1 );
    },
    submitFiles() {
      this.files.forEach(file => {
        let reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => {
          console.log(reader.result);
        };
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
          this.$refs.previewImg.getImagePreviews();
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