
<template>
<div class="detail col-sm-12 col-md-6" v-if="cliente">
  <form>
    <h3 v-if="cliente.Id" class="text-center">Modificando Cliente</h3>
    <h3 v-else class="text-center">Creando Cliente</h3>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="nombre-cliente" class="col-form-label">Nombre: </label></b>
        <input type="text" class="form-control" id="nombre-cliente" v-model="cliente.Nombre" />
      </div>
      <div class="form-group col-md-6">
        <b><label for="apellidos-cliente" class="col-form-label">Apellidos: </label></b>
        <input type="text" class="form-control" id="apellidos-cliente" v-model="cliente.Apellidos" />
      </div>
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="dni-cliente">Dni: </label></b>
        <input type="text" class="form-control" id="dni-cliente" v-model="cliente.Dni" />
      </div>
      <div class="form-group col-md-6">
        <b><label for="domicilio-cliente">Domicilio: </label></b>
        <input type="text" class="form-control" id="domicilio-cliente" v-model="cliente.Domicilio" />
      </div>
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="telefono-cliente">Teléfono: </label></b>
        <input type="text" class="form-control" id="telefono-cliente" v-model="cliente.Telefono" />
      </div>
      <div class="form-group col-md-6">
        <b><label for="fnac-cliente">Fecha de nacimiento: </label></b>
        <input type="datetime-local" class="form-control" id="fnac-cliente" v-model="cliente.FechaNacimiento" />
      </div>
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="fcarnet-cliente">Fecha de carné: </label></b>
        <input type="datetime-local" class="form-control" id="fcarnet-cliente" v-model="cliente.FechaCarnet" />
      </div>
      <div class="form-group col-md-6">
        <b><label for="puntos-cliente">Puntos de carné: </label></b>
        <input type="text" class="form-control" id="puntos-cliente" v-model="cliente.PuntosCarnet" />
      </div>
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="id-cliente">Código del cliente: </label></b>
        <input type="text" class="form-control" id="id-cliente" v-model="cliente.Id" />
      </div>
    </div>

    <div class="group-btn">
      <button v-if="cliente.Id" class="btn btn-success" @click="handleModificarCliente($event)">Modificar</button>
      <button v-else class="btn btn-success" @click="handleCrearCliente($event)">Crear</button>
      <button class="btn btn-secondary btn-cancelar" @click="handleCancelar($event)">Cancelar</button>
    </div>
  </form>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ClientesDetail',
  props: ['api_host'],
  
  data() {
    return {
      cliente: null,
      host: this.api_host
    };
  },

  created() {
    debugger;
    let _this = this;
    this.$parent.$on('show-form', (cliente) => {
      _this.cliente = cliente ? cliente : {
        Nombre: '',
        Apellidos: '',
        Dni: '',
        Domicilio: '',
        Telefono: '',
        FechaNacimiento: '',
        FechaCarnet: '',
        PuntosCarnet: ''
      };
      
    });

    Vue.$on('edit-cliente', (cliente) => {
      _this.cliente = cliente
    });

    Vue.$on('close-form', () => {
      _this.cliente = null
    });
  },

  methods: {
        
    /* HANDLE SELF EVENTS */
    handleModificarCliente(event) {
        
      debugger;
      event.preventDefault();
      var cliente = this.cliente;

      if(cliente.Nombre==""||cliente.Apellidos==""||cliente.Dni==""||cliente.Domicilio==""||cliente.Telefono==""||cliente.FechaNacimiento==""||cliente.FechaCarnet==""||cliente.PuntosCarnet==""){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Debe rellenar todos los campos antes de poder modificar el cliente. Por favor, revíselo');
      }    
      else{
        axios.put(this.host + '/' + cliente.Id, cliente)
        .then(response=> {
          
          Vue.$emit('show-modal', 'Cliente modificado', 'El cliente ha sido modificado con éxito');
          this.$emit('addCliente');
        }).catch((error) => {
          
          Vue.$emit('show-modal', error.message, error.stack)
        });
      } 
      
    },

    handleCrearCliente(event) {
      // debugger;
      event.preventDefault();
      var cliente = this.cliente;
      

      if(cliente.Nombre==""||cliente.Apellidos==""||cliente.Dni==""||cliente.Domicilio==""||cliente.Telefono==""||cliente.FechaNacimiento==""||cliente.FechaCarnet==""||cliente.PuntosCarnet==""){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Debe rellenar todos los campos antes de poder guardar. Por favor, revíselo');
      }
      
      else{
        axios.post(this.host, {
            Nombre: cliente.Nombre,
            Apellidos: cliente.Apellidos,
            Dni: cliente.Dni,
            Domicilio: cliente.Domicilio,
            Telefono:cliente.Telefono,
            FechaNacimiento:cliente.FechaNacimiento,
            FechaCarnet:cliente.FechaCarnet,
            PuntosCarnet:cliente.PuntosCarnet
          })
          .then(response=> {
            Vue.$emit('show-modal', 'Cliente creado', 'El nuevo cliente ha sido creado con éxito');
            this.$emit('addCliente');
          });

      }
      
      

    },

    handleCancelar(event) {
      event.preventDefault();
      this.cliente = null;
    }
  }
}
</script>

<style>
</style>
