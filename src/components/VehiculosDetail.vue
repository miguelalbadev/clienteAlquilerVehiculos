
<template>
<div class="detail col-sm-12 col-md-6" v-if="vehiculo">
  <form>
    <h3 v-if="vehiculo.Id" class="text-center">Modificando Vehículo</h3>
    <h3 v-else class="text-center">Creando Vehículo</h3>

    <!--<div class="form-group">
      <label for="nombre-tipo-evento">Nombre: </label>
      <input type="text" class="form-control" id="nombre-tipo-evento" v-model="tipoevento.Nombre" />
    </div>
    <div class="form-group">
      <label for="categoria-tipo-evento">Categoría: </label>
      <input type="text" class="form-control" id="categoria-tipo-evento" v-model="tipoevento.Categoria" />
    </div>
    <div class="form-group">
      <label for="criticidad-tipo-evento">Criticidad: </label>
      <input type="text" class="form-control" id="criticidad-tipo-evento" v-model="tipoevento.Criticidad" />
    </div>
    <div class="form-group">
      <label for="descripcion-tipo-evento">Descripción: </label>
      <input type="text" class="form-control" id="descripcion-tipo-evento" v-model="tipoevento.Descripcion" />
    </div>-->


    <div class="group-btn">
      <button v-if="vehiculo.Id" class="btn btn-success" @click="handleModificarEvento($event)">Modificar</button>
      <button v-else class="btn btn-success" @click="handleCrearTipoEvento($event)">Crear</button>
      <button class="btn btn-secondary" @click="handleCancelar($event)">Cancelar</button>
    </div>
  </form>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'VehiculosDetail',
  props: ['api_host'],
  
  data() {
    return {
      vehiculo: null,
      host: this.api_host
    };
  },

  created() {
    let _this = this;
    this.$parent.$on('show-form', (vehiculo) => {
      _this.vehiculo = vehiculo ? vehiculo : {
        Nombre: '',
        Apellidos: '',
        Dni: '',
        Domicilio: ''
      };
    });

    Vue.$on('edit-tipoevento', (tipoevento) => {
      _this.tipoevento = tipoevento
    });

    Vue.$on('close-form', () => {
      _this.tipoevento = null
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
      this.tipoevento = null;
    }
  }
}
</script>

<style>
</style>
