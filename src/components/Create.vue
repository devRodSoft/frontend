<template>
    <div>
        <h1>Crear Factura</h1>

        <b-form v-on:submit.prevent="onSubmit" @reset="onReset">
            <b-form-group>
                <label for="created">Receptor * en el blur cargar el UID del cliente si existe</label>
                <b-form-input v-model="form.Receptor.ResidenciaFiscal" @blur="client"></b-form-input>
                <label for="created">UID *</label>
                <b-form-input v-model="form.Receptor.UID"></b-form-input>
            </b-form-group>
            <b-form-group>
                <label for="">Tipo de Documento</label>
                <div>
                    <b-form-select class="custom-select" v-model="form.TipoDocumento" :options="documentType.data"></b-form-select>      
                </div>
            </b-form-group>
            <b-form-group>
                <label for="">Clave de Produto o Servicio</label>
                <b-form-input v-model="form.Conceptos[0].ClaveProdServ"></b-form-input>

                <label for="">No Identificacion </label>
                <b-form-input v-model="form.Conceptos[0].NoIdentificacion"></b-form-input>

                <label for="">Cantidad </label>
                <b-form-input v-model="form.Conceptos[0].Cantidad"></b-form-input>
                
                <label for="">Unidad</label>          
                <div>
                    <select id="unidades" v-model="form.Conceptos[0].ClaveUnidad">
                        <option v-for="item in listClaveUnidad" :key="item.index" :value="item.key">{{ item.name }}</option>
                    </select>
                </div>

                <label for="">Unidad </label>
                <b-form-input v-model="form.Conceptos[0].Unidad"></b-form-input>  

                <label for="">Descripcion</label>               
                <b-form-input v-model="form.Conceptos[0].Descripcion"></b-form-input>

                <label for="">ValorUnitario</label>               
                <b-form-input v-model="form.Conceptos[0].ValorUnitario"></b-form-input>

                <label for="">Importe</label>               
                <b-form-input v-model="form.Conceptos[0].Importe"></b-form-input>

                <label for="">Descuento</label>               
                <b-form-input v-model="form.Conceptos[0].Descuento"></b-form-input>

            </b-form-group>

            <b-form-group>
                <label for="">Base</label>               
                <b-form-input v-model="form.Conceptos[0].Impuestos.Translados.Base"></b-form-input>

                <label for="">Impuesto</label>               
                <b-form-input v-model="form.Conceptos[0].Impuestos.Impuesto"></b-form-input>

                <label for="">TipoFactor</label>               
                <b-form-input v-model="form.Conceptos[0].Impuestos.Translados.TipoFactor"></b-form-input>

                <label for="">TasaCuota</label>               
                <b-form-input v-model="form.Conceptos[0].Impuestos.TasaCuota"></b-form-input>
                
                <label for="">importe</label>               
                <b-form-input v-model="form.Conceptos[0].Impuestos.Translados.importe"></b-form-input>
            </b-form-group>

            <b-form-group>
                <label for="">Uso CFDI</label>          
                <div>
                    <select id="unidades" v-model="form.usoCFDI">
                        <option v-for="item in listCfdi" :key="item.index" :value="item.key">{{ item.name }}</option>
                    </select>
                </div>
            </b-form-group>

            <b-form-group>
                <label for="">Serie</label>               
                <b-form-input v-model="form.Serie"></b-form-input>

                <label for="">Metodo Pago</label>          
                <div>
                    <select id="unidades" v-model="form.MetodoPago">
                        <option v-for="item in listMetodoPago" :key="item.index" :value="item.key">{{ item.name }}</option>
                    </select>
                </div>

                <label for="">Condiciones de Pago</label>               
                <b-form-input v-model="form.CondicionesDePago"></b-form-input>

                <label for="">Moneda</label>          
                <div>
                    <select id="unidades" v-model="form.Moneda">
                        <option v-for="item in listMonedas" :key="item.index" :value="item.key">{{ item.name }}</option>
                    </select>
                </div>

                <label for="">Tipo Cambio</label>               
                <b-form-input v-model="form.TipoCambio"></b-form-input>

                <label for="">Fecha</label> 
                <b-form-datepicker id="example-datepicker" v-model="form.Fecha" locale="en"  today-button reset-button></b-form-datepicker>


                <label for="">Comentarios</label>               
                <b-form-input v-model="form.Comentarios"></b-form-input>

                <label for="">Cuenta</label>               
                <b-form-input v-model="form.Cuenta"></b-form-input>
                
                <label for="">Lugar Expedicion</label>               
                <b-form-input v-model="form.LugarExpedicion"></b-form-input>
            </b-form-group>

            <b-button type="submit" variant="primary">Submit</b-button>
        </b-form>
    </div>
</template>
<script>
import axios from "axios";

export default ({
    name: 'Create',
    data() {
      return {
          documentType: [],
          docSelected: null,
          listClaveUnidad: [],
          listCfdi: [],
          listMetodoPago: [],
          listMonedas: [],
          form: {
              Receptor: {
                ResidenciaFiscal: null,
                UID: '',
              },
              TipoDocumento: "factura",
              Conceptos: [
                  {
                    ClaveProdServ: '43232408',
                    NoIdentificacion: '0021',
                    Cantidad: '1.000000',
                    ClaveUnidad: 18,
                    Unidad: '',
                    Descripcion: 'Desarrollo web a la medida',
                    ValorUnitario: '15000.000000',
                    Importe: '15000.000000',
                    Descuento: '0',
                    Impuestos: {
                        Translados: [{
                            Base: '150000.00000',
                            Impuesto: '002',
                            TipoFactor: 'Tasa',
                            TasaCuota: '0.16',
                            importe: '24000.00000',
                        }],
                        Retenidos: [],
                        Locales: [],
                    }
                  }
              ],
              usoCFDI:null,
              Serie: null,
              MetodoPago: null,
              CondicionesDePago: "6 meses",
              Moneda: 'MXN',
              TipoCambio: null,
              Fecha: null,
              Comentarios: null,
              Cuenta: '4469',
              LugarExpedicion: '47180'
          }
      }
    },
    created() {
        axios.get('http://localhost:8000/api/docs/')
           .then(response => (this.documentType = response))
        
        axios.get('http://localhost:8000/api/unidad/')
           .then(response => (this.listClaveUnidad = response.data.data))
        
        axios.get('http://localhost:8000/api/cfdi/')
           .then(response => (this.listCfdi = response.data.data))
        
        axios.get('http://localhost:8000/api/mpago/')
           .then(response => (this.listMetodoPago = response.data.data))

        axios.get('http://localhost:8000/api/moneda/')
           .then(response => (this.listMonedas = response.data.data))
        
    },
    methods: {
        client: function () {
            axios.get('http://localhost:8000/api/client/'+this.form.Receptor.ResidenciaFiscal)
           .then(response => (this.form.Receptor.UID = response.data.Data.UID))
        },
        onSubmit: function() {
            this.client();
            var data = {
                receptor:  ['UID', this.form.Receptor.UID],
                TipoDocumento: this.form.TipoDocumento,
                usoCFDI: this.form.usoCFDI,
                redondeo: '2',
                Conceptos: this.form.Conceptos,
                FormaPago: this.form.FormaPago,
                MetodoPago: this.form.MetodoPago,
                Moneda: this.form.Moneda,
                CondicionesDePago: this.form.CondicionesDePago,
                Serie: this.form.Serie,
                EnviarCorreo:'true',
                InvoiceComments: this.form.Comentarios
            }

            axios.post('http://localhost:8000/api/create', data)
            .then(response => (console.log(response)))
        },
        onReset: function() {
            console.log('Reset');
        }
    }
})
</script>
