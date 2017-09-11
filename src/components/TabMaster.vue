<template>
  <div>
  <h2>Alquiler de Vehículos</h2>
      <ul class="nav nav-tabs" id="myTab" role="tablist">
      <li class="nav-item">
        <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-expanded="true">Vehículos</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" role="tab" aria-controls="profile">Clientes</a>
      </li>
      
    </ul>
    <div class="tab-content" id="myTabContent">
      <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">
        <VehiculosMaster></VehiculosMaster>
      </div>
      <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">
        <ClientesMaster></ClientesMaster>
      </div>
      
    </div>
  </div>
</template>

<script>
  import Tab from './Tab.vue';

  import VehiculosMaster from './VehiculosMaster.vue';
  import ClientesMaster from './ClientesMaster.vue';


  export default {
    name: 'tab-master',
    components: {
      Tab,
      VehiculosMaster,
      ClientesMaster
    },

    data() {
      return { tabs: [] };
    },

    created() {
      this.tabs = this.$children;
    },

    methods: {
      selectTab(selected_tab) {
        this.tabs.forEach((tab) => {
          tab.activated = (tab.href == selected_tab.href);
        });
        Vue.$emit('close-form');
      }
    }
  };
</script>
<style>
h2{
position:relative;
right:-2px;
padding:8px 63px 6px 17px;
width:100%;
margin:0;
font-size:32px;
background:#4F4F4F; /* Background color */
color:#f2f2f2; /* Color of the widget's title */
text-align:center;
text-indent:18px;
}
h2:before{
position:absolute;
content:"";
top:36px;
right:0px;
width: 0px;
height: 0px;
border-bottom:12px outset transparent;
border-left:12px solid #000000;
}
h2:after{
position:absolute;
content:"";
top:-6px;
left:-5px;
width: 0px;
height: 0px;
border-bottom:24px outset transparent;
border-top:24px outset transparent;
border-left:24px solid #ffffff; /* Arrow background color on the left */
}

</style>
