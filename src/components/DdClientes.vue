<template>
  <div class="container">
    <h2>Debida diligencia clientes</h2>
    <Tabla :datos="datos" :tabla="tabla"  :userlogued="userlogued" :endpoint="endpoint" :listas="listas" :endpointexport="endpointexport" />
  </div>
</template>
<script>
import Tabla from './Tabla.vue'
import axios from 'axios'
export default {

  components: {
    Tabla
  },
  mixins: [],
  props: {
    userlogued:{}
  },
  data() {
    return {
      show_table: false,
      datos: [],
      endpoint: 'consultaformulariocliente',
      endpointexport: 'exportaformulariocliente',
      URL_API: process.env.VUE_APP_URL_API,
      tabla: [
        { nombre: "#", orden: "DESC" },
        { nombre: "Nombre / Razón social", orden: "DESC", tipo: "texto", calculado: 'false' },
        { nombre: "Identificacion", orden: "DESC", tipo: "texto", calculado: 'false' },
        { nombre: "Nit", orden: "DESC", tipo: "texto", calculado: 'false' },
        { nombre: "Nombre del comercial", orden: "DESC", tipo: "texto", calculado: 'true' },
        { nombre: "Teléfono cliente", orden: "DESC", tipo: "texto", calculado: 'false' },
        { nombre: "Fecha registro", orden: "DESC", tipo: "fecha", calculado: 'false' },
      ],
      ejecutivos_comerciales:[],
      listas:[],
    }
  },
  computed: {

  },
  watch: {

  },
  mounted() {

  },
  created() {
    this.getItems()
    this.llenarLista()
    this.getEjecutivosComerciales()
  },
  methods: {
    llenarLista() {
      this.tabla.forEach((item) => {
        if (item.calculado == 'false') {
          this.listas.push([])
        }
      })
      this.listas.push([])
    },
    getItems() {
      let self = this;
      let config = this.configHeader();
      axios
        .get(self.URL_API + "api/v1/consultaformulariocliente/" + 10, config)
        .then(function (result) {
          self.datos = result;
        });
    },
    getEjecutivosComerciales() {
      let self = this;
      let config = this.configHeader();
      axios
        .get(self.URL_API + "api/v1/ejecutivocomerciallista", config)
        .then(function (result) {
          self.ejecutivos_comerciales = result.data
          self.listas.splice(4,0,result.data)
        });
    },
    configHeader() {
      let config = {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("access_token"),
        },
      };
      return config;
    },
  }
};
</script>
<style >
h2 {
    font-family: "Montserrat", sans-serif;
    margin: 20px 0px 20px 0px;
}
</style>