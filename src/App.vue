<template>
  <div>
    <drop-components />
  </div>
</template>s

<script>
import DropComponents from '@/components/DropComponents';

export default {
  name: 'App',
  components: {
    DropComponents
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.droppable {
  padding: 15px;
  height: 250px;
  border-radius: 5px;
  background: #2c3e50;
  margin-bottom: 10px;
}
</style>
