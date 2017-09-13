<template>
    <div class="master row">
    <div class="list col-sm-12 col-md-6">
        <button class="btn btn-primary btn-crear" @click="crearReserva">Crear nueva reserva</button>
        <div class="list-group">
            
            <a v-for="reserva in reservasList" href="#" class="list-group-item clearfix" v-on:click="selectReserva(reserva)">
                Fecha entrega: {{reserva.FechaRecogida}}  Lugar: {{reserva.LugarRecogida}}
                    <button class="btn btn-default float-right btn-editar" @click="handleEditarClick(reserva)">Editar</button>
                    <button class="btn btn-danger float-right btn-borrar" @click="handleBorrarClick(reserva)">Borrar</button>
                
            </a>
            
        </div>
    </div>
    <ReservasDetail @addReserva="cargaListadoReservas" @modifyEvento="onModifyEvento" :api_host="host"></ReservasDetail>
    </div>

</template>

<script>
import axios from 'axios';
import ReservasDetail from './ReservasDetail.vue';

export default {
    name: 'ReservasMaster',
    components: {
        ReservasDetail
    },
    data:function(){
        return {
            reservasList:[],
            host: 'http://localhost:50283/api/Reservas',

        }
    },
    mounted() {
        this.cargaListadoReservas();
    },
    methods: {
        selectReserva: function(reserva) {
        debugger;
        this.$emit('show-form', reserva);
        },
        cargaListadoReservas(){
            // debugger;

            let _this = this;
            _this.reservasList.length = 0;
            axios.get(this.host).then((response) => {
              _this.reservasList = response.data;
            }).catch((error) => {
              //Vue.$emit('show-modal', error.message, error.stack)
            });
        },
        crearReserva(){
            this.$emit('show-form', null);
        },
        handleEditarClick(reserva){
            Vue.$emit('edit-reserva',reserva);
        },
        handleBorrarClick(reserva){

             axios.delete(this.host + '/' + reserva.Id)
                .then((res) => {
                  Vue.$emit('show-modal', 'Reserva eliminada', 'La reserva ha sido eliminada con éxito');
                  this.cargaListadoReservas();
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
