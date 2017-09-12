<template>
    <div class="master row">
    <div class="list col-sm-12 col-md-6">
        <button class="btn btn-primary btn-crear" @click="crearVehiculo">Crear nuevo vehículo</button>
        <div class="list-group">
            
            <a v-for="vehiculo in vehiculosList" href="#" class="list-group-item clearfix" v-on:click="selectVehiculo(vehiculo)">
                {{vehiculo.Marca}}  {{vehiculo.Modelo}}
                    <button class="btn btn-default float-right btn-editar" @click="handleEditarClick(vehiculo)">Editar</button>
                    <button class="btn btn-danger float-right btn-borrar" @click="handleBorrarClick(vehiculo)">Borrar</button>
                
            </a>
            
        </div>
    </div>
    <VehiculosDetail @addVehiculo="cargaListadoVehiculos" @modifyEvento="onModifyEvento" :api_host="host"></VehiculosDetail>
    </div>
    
</template>

<script>
import axios from 'axios';
import VehiculosDetail from './VehiculosDetail.vue';

export default {
    name: 'VehiculosMaster',
    components: {
        VehiculosDetail
    },
	data:function(){
		return {
			vehiculosList:[],
            host: 'http://localhost:50283/api/Vehiculos',

		}
	},
    mounted() {
        this.cargaListadoVehiculos();
    },
    methods: {
        selectVehiculo: function(vehiculo) {
        debugger;
        
        this.$emit('show-form', vehiculo);
        },
        cargaListadoVehiculos(){
            debugger;

            let _this = this;
            _this.vehiculosList.length = 0;
            axios.get(this.host).then((response) => {
              _this.vehiculosList = response.data;
            }).catch((error) => {
              //Vue.$emit('show-modal', error.message, error.stack)
            });
        },
        crearVehiculo(){
            this.$emit('show-form', null);
        },
        handleEditarClick(vehiculo){
            Vue.$emit('edit-vehiculo',vehiculo);
        },
        handleBorrarClick(vehiculo){

             axios.delete(this.host + '/' + vehiculo.Id)
                .then((res) => {
                  Vue.$emit('show-modal', 'Vehículo eliminado', 'El vehículo ha sido eliminado con éxito');
                  this.cargaListadoVehiculos();
                  Vue.$emit('close-form');
             })

        },
        onAddTipoEvento(tipoEvento){

        },
        onModifyEvento(tipoevento){

        }

    }

}
</script>

<style>

.list-group-item span {
	display: block;
	padding: 10px;
	max-width: 100%;
	word-wrap: break-word;
}

.master-button-group {
	padding-left: 30px;
	padding-right: 0px;
}

.master-button-group button {
	width: 75px;
	margin-left: 5px;
	margin-bottom: 5px;
}

.master {
	padding: 20px;
}

.btn-crear {
	width: 100%;
	margin: 5px 0 5px 0;
}

.btn-editar {
    margin-left: 15px;
}

.btn-borrar {
    
}


</style>
