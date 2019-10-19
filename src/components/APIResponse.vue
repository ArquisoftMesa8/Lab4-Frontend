<template>
  <div class="hello">
    <h3 v-bind = "data" >  {{ data.msg }}</h3>
    <h4  v-bind = "data"> Código:  {{ data.code }} </h4>
    <h4  v-bind = "data"> Nombre:  {{ data.name }} </h4>
    <h4  v-bind = "data"> Créditos: {{ data.credits }} </h4>
  </div>
</template>

<script>
export default {
  name: 'APIResponse',
  props: {
    data: Object
  }
}
</script>


<style scoped>
h3 {
  margin: 40px 0 0;
}
h3 {
  margin: 20px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>


