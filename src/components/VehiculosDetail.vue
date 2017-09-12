
<template>
<div class="detail col-sm-12 col-md-6" v-if="vehiculo">
  <form>
    <h3 v-if="vehiculo.Id" class="text-center">Modificando Vehículo</h3>
    <h3 v-else class="text-center">Creando Vehículo</h3>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="marca-vehiculo" class="col-form-label">Marca: </label></b>
        <input type="text" class="form-control" id="marca-vehiculo" v-model="vehiculo.Marca" />
      </div>
      <div class="form-group col-md-6">
        <b><label for="modelo-vehiculo" class="col-form-label">Modelo: </label></b>
        <input type="text" class="form-control" id="modelo-vehiculo" v-model="vehiculo.Modelo" />
      </div>
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="tipo-vehiculo">Tipo de vehículo: </label></b>
        <select class="custom-select mb-2 mr-sm-2 mb-sm-0 form-control" id="inlineFormCustomSelect" v-model="vehiculo.TipoVehiculo">
          <option disabled value="">Choose...</option>
          <option>Berlina</option>
          <option>Compacto</option>
          <option>Utilitario</option>
        </select>
      </div>
      <div class="form-group col-md-6">
        <b><label for="combustible-vehiculo">Tipo de combustible: </label></b>
        <select class="custom-select mb-2 mr-sm-2 mb-sm-0 form-control" id="inlineFormCustomSelect" v-model="vehiculo.TipoCombustible">
          <option disabled value="">Choose...</option>
          <option>Gasolina</option>
          <option>Diesel</option>
          <option>Eléctrico</option>
          <option>Híbrido</option>
        </select>
      </div>
      
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="cambio-vehiculo">Tipo de cambio: </label></b>
        <select class="custom-select mb-2 mr-sm-2 mb-sm-0 form-control" id="inlineFormCustomSelect" v-model="vehiculo.TipoCambio">
          <option disabled value="">Choose...</option>
          <option>Manual</option>
          <option>Automático</option>
          
        </select>
      </div>
      <div class="form-group col-md-6">
        <b><label for="plazas-vehiculo">Número de plazas: </label></b>
        <input type="text" class="form-control" id="plazas-vehiculo" v-model="vehiculo.NumeroPlazas" />
      </div>
      
    </div>
    <div class="form-row">
      <div class="form-group col-md-6">
        <b><label for="ac-vehiculo">Aire acondicionado: </label></b>
        <select class="custom-select mb-2 mr-sm-2 mb-sm-0 form-control" id="inlineFormCustomSelect" v-model="aire">
          <option disabled value="">Choose...</option>
          <option>Si</option>
          <option>No</option>
          
        </select>
      </div>
      <div class="form-group col-md-6">
        <b><label for="tarifa-vehiculo">Tarifa diaria: </label></b>
        <input type="text" class="form-control" id="tarifa-vehiculo" v-model="vehiculo.TarifaDiaria" />
      </div>
    </div>

    <div class="group-btn">
      <button v-if="vehiculo.Id" class="btn btn-success" @click="handleModificarVehiculo($event)">Modificar</button>
      <button v-else class="btn btn-success" @click="handleCrearVehiculo($event)">Crear</button>
      <button class="btn btn-secondary btn-cancelar" @click="handleCancelar($event)">Cancelar</button>
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
      aire: null,
      host: this.api_host
    };
  },

  created() {
    let _this = this;
    this.$parent.$on('show-form', (vehiculo) => {
      _this.vehiculo = vehiculo ? vehiculo : {
        Marca: '',
        Modelo: '',
        TipoVehiculo: '',
        TipoCombustible: '',
        NumeroPlazas: '',
        TipoCambio: '',
        AireAcondicionado: '',
        TipoCombustible: '',
        TarifaDiaria: ''
      };
      if(vehiculo!=null){
        if(vehiculo.AireAcondicionado==true){
        _this.aire = 'Si';
        }
        else{
          _this.aire = 'No';
        }
      }
      
    });

    Vue.$on('edit-vehiculo', (vehiculo) => {
      _this.vehiculo = vehiculo
    });

    Vue.$on('close-form', () => {
      _this.vehiculo = null
    });
  },

  methods: {
    
    /* HANDLE SELF EVENTS */
    handleModificarVehiculo(event) {
        debugger;
        event.preventDefault();
        var vehiculo = this.vehiculo;
        var aire;
        if(vehiculo.AireAcondicionado=='Si'){
          aire = true;
        }
        else if(vehiculo.AireAcondicionado=='No'){
          aire = false;
        }
        axios.put(this.host + '/' + vehiculo.Id, vehiculo)
          .then(response=> {
            
            Vue.$emit('show-modal', 'Vehículo modificado', 'El vehículo ha sido modificado con éxito');
            this.$emit('addVehiculo');
          }).catch((error) => {
            
            Vue.$emit('show-modal', error.message, error.stack)
          });

        
    },

    handleCrearVehiculo(event) {
      debugger;
      event.preventDefault();
      var vehiculo = this.vehiculo;
      var aire = this.aire;
      //var res = new RegExp('^[0-5]{1}$');
      //var found = tipo.Criticidad.match(res);

      if(vehiculo.Marca==""||vehiculo.Modelo==""||vehiculo.TipoVehiculo==""||vehiculo.TipoCombustible==""||vehiculo.NumeroPlazas==""||
        vehiculo.TipoCambio==""||this.aire==null||vehiculo.TarifaDiaria==""){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Debe rellenar todos los campos antes de poder guardar. Por favor, revíselo');
      }
      /*else if(found==null){
        Vue.$emit('show-modal', 'Guardado no permitido', 'El campo Criticidad debe ser un valor numérico entre 0 y 5');
      }*/
      else{
        
        if(aire=='Si'){
          vehiculo.AireAcondicionado=true;
        }
        else if(aire=='No'){
          vehiculo.AireAcondicionado=false;
        }
        axios.post(this.host, {
            Marca: vehiculo.Marca,
            Modelo: vehiculo.Modelo,
            TipoVehiculo: vehiculo.TipoVehiculo,
            TipoCombustible: vehiculo.TipoCombustible,
            NumeroPlazas: vehiculo.NumeroPlazas,
            TipoCambio: vehiculo.TipoCambio,
            AireAcondicionado: vehiculo.AireAcondicionado,
            TarifaDiaria: vehiculo.TarifaDiaria
          })
          .then(response=> {
            Vue.$emit('show-modal', 'Vehículo creado', 'El nuevo vehículo ha sido creado con éxito');
            this.$emit('addVehiculo');
          });

      }
        
      

    },

    handleCancelar(event) {
      event.preventDefault();
      this.vehiculo = null;
    }
  }
}
</script>

<style>
.btn-cancelar{
  margin-left: 45px;
}
</style>
