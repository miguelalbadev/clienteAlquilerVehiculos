
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

    <div class="group-btn">
      <button v-if="cliente.Id" class="btn btn-success" @click="handleModificarEvento($event)">Modificar</button>
      <button v-else class="btn btn-success" @click="handleCrearTipoEvento($event)">Crear</button>
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
    /* SERVER REQUESTS */
    updateEvento() {

    },

    createEvento() {

    },

    /* HANDLE SELF EVENTS */
    handleModificarEvento(event) {
        // debugger;
        event.preventDefault();
        var tipo = this.tipoevento;
        axios.put(this.host + '/' + tipo.Id, {
            Id: tipo.Id,
            Nombre: tipo.Nombre,
            Categoria: tipo.Categoria,
            Criticidad: tipo.Criticidad,
            Descripcion: tipo.Descripcion
        },{headers:{"Content-Type":"application/json"}})
          .then(response=> {
            Vue.$emit('show-modal', 'Tipo de Evento modificado', 'El Tipo de Evento ha sido modificado con éxito');
            this.$emit('addTipoEvento');
          }).catch((error) => {
            // debugger;
              //Vue.$emit('show-modal', error.message, error.stack)
          });

        this.tipoevento = null;
    },

    handleCrearTipoEvento(event) {
      // debugger;
      event.preventDefault();
      var tipo = this.tipoevento;
      var res = new RegExp('^[0-5]{1}$');
      var found = tipo.Criticidad.match(res);

      if(tipo.Nombre==""||tipo.Categoria==""||tipo.Criticidad==""||tipo.Descripcion==""){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Debe rellenar todos los campos antes de poder guardar. Por favor, revíselo');
      }
      else if(found==null){
        Vue.$emit('show-modal', 'Guardado no permitido', 'El campo Criticidad debe ser un valor numérico entre 0 y 5');
      }
      else{
        axios.post(this.host, {
            Nombre: tipo.Nombre,
            Categoria: tipo.Categoria,
            Criticidad: tipo.Criticidad,
            Descripcion: tipo.Descripcion
          })
          .then(response=> {
            Vue.$emit('show-modal', 'Tipo de evento creado', 'El nuevo tipo de evento ha sido creado con éxito');
            this.$emit('addTipoEvento');
          });

      }
        
      this.tipoevento = null;

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
