<template>
	<div>
	  	<!--<div class="list col-sm-12 col-md-6">-->
        <button class="btn btn-primary" @click="crearTipoEvento">Crear nuevo vehículo</button>
        <!--<div class="list-group">
        <a v-for="vehiculo in vehiculosList" class="list-group-item clearfix">
            <span class="col-sm-8">{{vehiculo.Marca}}  {{vehiculo.Modelo}}</span>
			<div class="master-button-group col-sm-12 col-md-4">
				<button class="btn btn-default " @click="handleEditarClick(tipoevento)">Editar</button>
				<button class="btn btn-danger " @click="handleBorrarClick(tipoevento)">Borrar</button>
			</div>
        </a>
        </div>-->

		<!--</div>-->
        <VehiculosDetail @addTipoEvento="cargaListadoVehiculos" @modifyEvento="onModifyEvento" :api_host="host"></VehiculosDetail>
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
        selectTipoEvento: function(tipoEvento) {
            // debugger;
        //this.seen=true;
        //this.$emit('selectEntrada', entrada);
        },
        cargaListadoVehiculos(){
            debugger;

            let _this = this;
            _this.vehiculosList.length = 0;
            
            $.ajax({
                url: _this.host,
                type: 'GET',
                // el tipo de información que se espera de respuesta
                dataType: 'json',
                // código a ejecutar si la petición es satisfactoria;
                // la respuesta es pasada como argumento a la función
                success: function(data) {
                  debugger;
                  _this.vehiculosList = data;
                },
                error:function(xhr, status) {
                   debugger;
                },
                // código a ejecutar sin importar si la petición falló o no
                complete: function(xhr, status) {
                  //alert('Petición realizada');
                }
            });
        },
        crearTipoEvento(){
            Vue.$emit('show-form', null);
        },
        handleEditarClick(tipoevento){
            Vue.$emit('edit-tipoevento',tipoevento);
        },
        handleBorrarClick(tipoevento){

             axios.delete(this.host + '/' + tipoevento.Id)
                .then((res) => {
                  Vue.$emit('show-modal', 'Tipo de Evento eliminado', 'El Tipo de Evento ha sido eliminado con éxito');
                  this.cargaListadoTipoEventos();
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
</style>
