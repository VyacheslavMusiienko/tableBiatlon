<template>
  <div id="container">
    <the-table /> 
  </div>
</template>

<script>
import TheTable from './page/TheTable.vue'


export default {
  components: { TheTable },
  name: 'App',
  
}
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0
}
#container {
  max-width: 1170px;
  margin: auto;
  height: auto;
}

</style>
