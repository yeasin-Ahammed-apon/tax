<template>
  <div class="pe-6 ps-5 me-5 ms-5">
      <table class="table">
        <thead>
          <tr>            
            <th scope="col">Lable</th>
            <th scope="col">Status</th>
            <th scope="col">Value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(tax,index) in taxs" :key="index">            
            <td v-if="tax.is_selected == true" >{{tax.label}}</td>
            <td v-else class="text-decoration-line-through">{{tax.label}}</td>
            <td>              
              <div class="form-check form-switch" @click="status(index)">
                <input class="form-check-input" type="checkbox" id="flexSwitchCheckChecked"  checked  v-if="tax.is_selected == true">                
                <input class="form-check-input" type="checkbox" id="flexSwitchCheckChecked"   v-else-if="tax.is_selected == false">                
              </div>
            </td>
            <td>
              <div class="mb-3">                                
                  <input type="text" class="form-control"   @input="tax_value($event,index)" :value="tax.value"  v-if="tax.is_selected == true">  
                  <input type="text" class="form-control"   :value="tax.value" v-else disabled>              
              </div>
            </td>
          </tr>          
        </tbody>
      </table>    
  </div>
</template>
<script>
const axios = require('axios');
export default {
  name: 'Home',
  components: {

  },
  data() {
    return {
      taxs:[],    
      val:''        

      
    }
  },
  mounted () {          
      axios.get('https://dev-api.txbd.win/api/returns/document/132?return_document_type=tax_paid',{
         headers: {
           'Authorization':`Bearer ${localStorage.getItem('token')}`       
          }
      })
        .then((response)=>{          
          this.taxs = response.data.data;          
        })
        .catch(function (error) {        
          console.log(error);
        })        
  },
  methods: {
    status(index) {
      if(this.taxs[index].is_selected==true){
        this.taxs[index].is_selected = false        
      }else{
        this.taxs[index].is_selected=true        
      }      
       axios.post('https://dev-api.txbd.win/api/returns/document/132',{
                'return_document': JSON.stringify(this.taxs),
                'return_document_type': "tax_paid",
                'status': "completed"             
            },{
          headers: {            
            'Authorization':`Bearer ${localStorage.getItem('token')}`,
          }
          },)
            .then((response)=> {                          
                this.taxs = response.data.data
            })
            .catch((error)=>{              
                console.log(error);
            });
    },
    tax_value(event,index){    
      this.val = event.target.value            
      this.taxs[index].value = this.val.toString();                  
      axios.post('https://dev-api.txbd.win/api/returns/document/132',{
                'return_document': JSON.stringify(this.taxs),
                'return_document_type': "tax_paid",
                'status': "completed"             
            },{
          headers: {            
            'Authorization':`Bearer ${localStorage.getItem('token')}`,
          }
          },)
            .then((response)=> {                          
                this.taxs = response.data.data                                
            })
            .catch((error)=>{              
            console.log(error);
            });      
    }    
  },
}
</script>
