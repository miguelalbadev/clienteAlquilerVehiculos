<template>
<div class="detail col-sm-12 col-md-6" v-if="reserva">
  <form>
    <h3 v-if="reserva.Id" class="text-center">Modificando Reserva</h3>
    <h3 v-else class="text-center">Creando Reserva</h3>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="fecharec-reserva" class="col-form-label">Fecha de recogida: </label></b>
        <input type="datetime-local" class="form-control" id="nombre-reserva" v-model="reserva.FechaRecogida" />
      </div>
      <div class="form-group col-md-6">
        <b><label for="fechadev-reserva" class="col-form-label">Fecha de devolución: </label></b>
        <input type="datetime-local" class="form-control" id="fechadev-reserva" v-model="reserva.FechaDevolucion" />
      </div>
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="recog-reserva">Lugar de recogida: </label></b>
        <input type="text" class="form-control" id="recog-reserva" v-model="reserva.LugarRecogida" />
      </div>
      <div class="form-group col-md-6">
        <b><label for="entrega-reserva">Lugar de entrega: </label></b>
        <input type="text" class="form-control" id="entrega-reserva" v-model="reserva.LugarEntrega" />
      </div>
    </div>
    <div v-if="reserva.Id">
        <p><u><b>Datos del cliente:</b></u></p>
        <div class="form-row">
          <div class="form-group col-md-6">
            <b><label for="nombre-cliente">Nombre: </label></b>
            <input type="text" class="form-control" id="nombre-cliente" v-model="reserva.Cliente.Nombre" />
          </div>
          <div class="form-group col-md-6">
            <b><label for="apellidos-cliente">Apellidos: </label></b>
            <input type="text" class="form-control" id="apellidos-cliente" v-model="reserva.Cliente.Apellidos" />
          </div>
        </div>
        <p><u><b>Datos del vehículo:</b></u></p>
        <div class="form-row">
          <div class="form-group col-md-6">
            <b><label for="marca-vehiculo">Marca: </label></b>
            <input type="text" class="form-control" id="marca-vehiculo" v-model="reserva.Vehiculo.Marca" />
          </div>
          <div class="form-group col-md-6">
            <b><label for="modelo-vehiculo">Modelo: </label></b>
            <input type="text" class="form-control" id="modelo-vehiculo" v-model="reserva.Vehiculo.Modelo" />
          </div>
        </div>
    </div>

    <div v-else>
      <p><u><b>Datos del cliente:</b></u></p>
        <div class="form-row">
          <div class="form-group col-md-6">
            <b><label for="id-cliente">Código del cliente: </label></b>
            <input type="text" class="form-control" id="id-cliente" v-model="reserva.ClienteId" />
          </div>
        </div>
      <p><u><b>Datos del vehículo:</b></u></p>
        <div class="form-row">
          <div class="form-group col-md-6">
            <b><label for="id-vehiculo">Código del vehículo: </label></b>
            <input type="text" class="form-control" id="id-vehiculo" v-model="reserva.VehiculoId" />
          </div>
        </div>

    </div>


    <div class="group-btn">
      <button v-if="reserva.Id" class="btn btn-success" @click="handleModificarReserva($event)">Modificar</button>
      <button v-else class="btn btn-success" @click="handleCrearReserva($event)">Crear</button>
      <button class="btn btn-secondary btn-cancelar" @click="handleCancelar($event)">Cancelar</button>
    </div>
  </form>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ReservasDetail',
  props: ['api_host'],
  
  data() {
    return {
      reserva: null,
      host: this.api_host
    };
  },

  created() {
    debugger;
    let _this = this;
    this.$parent.$on('show-form', (reserva) => {
      _this.reserva = reserva ? reserva : {
        FechaRecogida: '',
        FechaDevolucion: '',
        LugarRecogida: '',
        LugarEntrega: ''
      };
      
    });

    Vue.$on('edit-reserva', (reserva) => {
      _this.reserva = reserva
    });

    Vue.$on('close-form', () => {
      _this.reserva = null
    });
  },

  methods: {
        
    /* HANDLE SELF EVENTS */
    handleModificarReserva(event) {
        
      debugger;
      event.preventDefault();
      var reserva = this.reserva;

      if(reserva.FechaRecogida==""||reserva.FechaDevolucion==""||reserva.LugarRecogida==""||reserva.LugarEntrega==""){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Debe rellenar todos los campos antes de poder modificar la reserva. Por favor, revíselo');
      }    
      else{
        axios.put(this.host + '/' + reserva.Id, reserva)
        .then(response=> {
          
          Vue.$emit('show-modal', 'Reserva modificada', 'La reserva ha sido modificada con éxito');
          this.$emit('addReserva');
        }).catch((error) => {
          
          Vue.$emit('show-modal', error.message, error.stack)
        });
      } 
      
    },

    handleCrearReserva(event) {
      // debugger;
      event.preventDefault();
      var reserva = this.reserva;
      

      if(reserva.FechaRecogida==""||reserva.FechaDevolucion==""||reserva.LugarRecogida==""||reserva.LugarEntrega==""){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Debe rellenar todos los campos antes de poder modificar la reserva. Por favor, revíselo');
      }   
      
      else{
        axios.post(this.host, reserva)
          .then(response=> {
            Vue.$emit('show-modal', 'Reserva creada', 'La nueva reserva ha sido creada con éxito');
            this.$emit('addReserva');
          });

      }
      
      

    },

    handleCancelar(event) {
      event.preventDefault();
      this.reserva = null;
    }
  }
}
</script>

<style>
</style>
