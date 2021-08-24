<template>
  <div id="app">
   
      <CustomersList/>
  </div>
</template>


<script>
import CustomersList from './components/CustomersList.vue'
import 'bootstrap-css-only/css/bootstrap.min.css'; 
import { ClientTable } from 'vue-tables-2';
import Vue from 'vue'

Vue.use(ClientTable);


export default {
  name: 'App',
  components: {
    CustomersList
  }
}
</script>


