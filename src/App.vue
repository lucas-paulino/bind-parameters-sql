<template>
  <div>
    <BindComponent msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
  import BindComponent from './components/BindComponent.vue'
  import "materialize-css/dist/css/materialize.css"
  import "font-awesome/css/font-awesome.css"
  
  export default {
    name: 'app',
    components: {
      BindComponent
    }
  }
</script>

<style>

</style>
