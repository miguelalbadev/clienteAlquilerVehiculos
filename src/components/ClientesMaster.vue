<template>
    <div class="master row">
    <div class="list col-sm-12 col-md-6">
        <button class="btn btn-primary btn-crear" @click="crearTipoEvento">Crear nuevo cliente</button>
        <div class="list-group">
            
            <a v-for="cliente in clientesList" href="#" class="list-group-item clearfix">
                {{cliente.Nombre}}  {{cliente.Apellidos}}
                    <button class="btn btn-default float-right btn-editar" @click="handleEditarClick(tipoevento)">Editar</button>
                    <button class="btn btn-danger float-right btn-borrar" @click="handleBorrarClick(tipoevento)">Borrar</button>
                
            </a>
            
        </div>
    </div>
    <ClientesDetail @addTipoEvento="cargaListadoClientes" @modifyEvento="onModifyEvento" :api_host="host"></ClientesDetail>
    </div>

</template>

<script>
import axios from 'axios';
import ClientesDetail from './ClientesDetail.vue';

export default {
    name: 'ClientesMaster',
    components: {
        ClientesDetail
    },
	data:function(){
		return {
			clientesList:[],
            host: 'http://localhost:50283/api/Clientes',

		}
	},
    mounted() {
        this.cargaListadoClientes();
    },
    methods: {
        selectTipoEvento: function(tipoEvento) {
            // debugger;
        //this.seen=true;
        //this.$emit('selectEntrada', entrada);
        },
        cargaListadoClientes(){
            // debugger;

            let _this = this;
            _this.clientesList.length = 0;
            axios.get(this.host).then((response) => {
              _this.clientesList = response.data;
            }).catch((error) => {
              //Vue.$emit('show-modal', error.message, error.stack)
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
