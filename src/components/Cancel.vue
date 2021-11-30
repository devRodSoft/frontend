<template>
    <div>
        <b-button @click="modalShow = !modalShow" variant="danger" title="Alerta">Cancelar</b-button>
        <b-modal @ok="handleOk" v-model="modalShow">Seguro que quieres cancelar la factura {{cancelId}}</b-modal>
        <b-modal v-model="modalMsg" title="Alerta">Factura Cancelada {{cancelId}}</b-modal>
    </div>
</template>
<script>

import axios from 'axios'

export default ({
    name: 'Cancel',
    data() {
      return {
        modalShow: false,    
        modalMsg: false    
      }
    },
    props: ['cancelId'],
    methods: {
        show: function () {
            console.log('show');
            this.modalMsg = true;
        },
        handleOk: function cancel(){            
            axios.get('http://localhost:8000/api/cancel/' +  this.$props.cancelId)
           .then(response => (console.log(response), this.show()))
        }
    }
})
</script>
