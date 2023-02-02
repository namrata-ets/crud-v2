<template>
  <v-container>
   
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios';
  

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
    airlineData:[] as string[],
    passData:[] as string[],
    airlineName:[] as string[],
    reqObj : {
      name:'John Doe',
      trips:250,
      airline: 5  
    }
  }
  },
  methods:{
    async addPassenger(){
      //console.log(this.pname);
      await axios.post('https://api.instantwebtools.net/v1/passenger',this.reqObj).then((response: { data: any; })=>{
        console.log(response.data)
      }).catch((error: any)=>console.log(error));
      
    },
  async getData(){
   await axios.get(
    `https://api.instantwebtools.net/v1/passenger?page=3862&size=10`
  ).then((response: { data: { data: never[]; }; })=>this.airlineData=response.data.data)
  .catch((error: any)=>console.log(error))
  console.log(this.airlineData);


  },
  async deletePassenger(id: string){
    await axios.delete('https://api.instantwebtools.net/v1/passenger/'+id).then(() => {
         this.getData();
         alert("successfully deleted the passenger record");  
      });
  },
  async updatePassenger(id: string){
   
    var airlinename:string='';
    var passid:string='';

    await axios.get(
    `https://api.instantwebtools.net/v1/passenger/`+id
  ).then((response: { data: never[]; })=>this.passData=response.data)
  .catch((error: any)=>console.log(error))
  console.log(this.passData);

    this.reqObj.name=this.passData.name;
    this.reqObj.trips=this.passData.trips;
    passid=this.passData._id;

    

    await axios.post('https://api.instantwebtools.net/v1/passenger/'+passid,this.reqObj).then((response: { data: string; })=>{
        console.log("Passenger data updated successfully"+response.data)
      }).catch((error: any)=>console.log(error));
  }
  },

 mounted(){
  this.getData();
  }  
}
</script>
