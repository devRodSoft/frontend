<template>
    <div>
        <b-button @click="modalShow = !modalShow" variant="success" title="Alerta">Enviar</b-button>
        <b-modal @ok="handleOk" v-model="modalShow">Envior correo a {{sendId}}</b-modal>

        <b-modal v-model="modalMsg" title="Alerta">Factura Enviada  {{sendId}}</b-modal>
    </div>
</template>
<script>

import axios from 'axios'

export default ({
    name: 'Send',
    data() {
      return {
        modalShow: false,  
        modalMsg: false      
      }
    },
    props: ['sendId'],
    methods: {
        show: function () {
            console.log('show');
            this.modalMsg = true;
        },
        handleOk: function cancel(){            
            axios.get('http://localhost:8000/api/send/' +  this.$props.sendId)
           .then(response => (console.log(response), this.show()))
        }
    }
})
</script>