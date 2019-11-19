<template>
  <div>
    <div class="row">
			<div class="col s10 offset-s1 m6 offset-m3 center-align">
        <h4>Unifica Paramentros e SQL</h4>
        <h6>Unifica paramentros ao SQL utilizando Vue, Node, Materialize</h6>
			</div>
    </div>
    <div id="main">
      <BindComponent/>
    </div>
  </div>
</template>

<script>
  import BindComponent from './components/BindComponent.vue'
  import "materialize-css/dist/css/materialize.css"
  
  export default {
    name: 'app',
    components: {
      BindComponent
    }
  }
</script>

<style>
  #slogan{
    margin: 30px 0px;
  }
  #main{
    background-color: #f1f1f1;
    padding: 30px 0px;
  }
</style>
