<template>
    <div class="container">
        <h1>Lista</h1>
        <b-table responsive    :fields="fields" :items="invoices"  class="mt-3" outlined>
       
          <template #cell(Actions)="data">
            <cancel v-bind:cancel-id="data.item.UID"></cancel>
             <send v-bind:send-id="data.item.UID"></send>
          </template>
          
          <template #table-busy>
            <div class="text-center text-danger my-2">
              <b-spinner class="align-middle"></b-spinner>
              <strong>Loading...</strong>
            </div>
          </template>
        </b-table>
        
        <create></create>
    </div>

    
</template>

<script>
import axios from "axios";
import Cancel from './Cancel';
import Send from './send';
import Create from './Create'

export default ({
  name: 'List',
  components: {
    Cancel,
    Send,
    Create
  },
  data () {
      return {
          isBusy: true,
          invoices: [],
          fields:  [
            //'Razon Social Receptor',
            'Folio',
            //'UID',
            'UUID',
            //'Subtotal',
            //'Descuento',
            'Total',
            'ReferenceClient',
            //'NumOrder',
            //'Receptor',
            'FechaTimbrado',
            'Status',
            //'Version '
            'Actions'
          ]
      }
  },
  methods: {
    toggleBusy() {
      this.isBusy = !this.isBusy
    }
  },
  created() {
      axios.get('http://localhost:8000/api/invoices')
           .then(response => (this.invoices = response.data.data, this.toggleBusy()))
  }


})
</script>