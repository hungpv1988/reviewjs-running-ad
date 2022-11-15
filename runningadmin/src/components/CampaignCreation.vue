<template>
    <div class="container" id="camp-panel">
        <form>
            <div class="row">
                <div class="col-md-2"> <label>Name</label> </div>
                <div class="col-md-4"> <input class="form-control" v-model="name" type="text"/> </div>
            </div>
            <div class="row">
                <div class="col-md-2"> <label>Des</label> </div>
                <div class="col-md-4">  <input class="form-control" v-model="des" type="text"/>  </div>
            </div>
            <div class="row">
                <div class="col-md-2"> <label>Start Location</label> </div>
                <div class="col-md-4"> <input class="form-control" v-model="startLocation" type="text"/>  </div>
            </div>
            <div class="row">
                <div class="col-md-2"> <label>logo</label> </div>
                <div class="col-md-4"> <input class="form-control" @change="handleFileUpload" type="file" id="formFile"> </div>
            </div>
            <div class="row">
                <div class="col-md-2"> <label>Date</label> </div>
                <div class="col-md-4">  <Datepicker v-model="mydate" :format="formatDate" /> </div>
            </div>
            
            <div class="row">
                 <div class="col-md-2">
                        your action
                </div>
                <div class="col-md-2">
                        <button @click.prevent="submitData">Submit</button>
                </div>
            </div>
        </form>
    </div>
</template>

<style scoped>
#camp-panel .row{
    margin-top: 10px;
}
</style>
<script setup>
import {ref} from 'vue';
import Datepicker from '@vuepic/vue-datepicker';
import axios from "axios";  
let mydate = ref(new Date());
let file = ref(null);
let name = ref("");
let des = ref("");
let startLocation = ref("");

const formatDate = (date) =>{
    const day = date.getDate();
    const month = date.getMonth() + 1;
    const year = date.getFullYear();

    return `${year}/${month}/${day}`;
};

function handleFileUpload(event){
    file = event.target.files[0];
};

function  submitData(event){
    let formData = new FormData();
    formData.append("campaignName", name.value);
    formData.append("description", des.value);
    formData.append("startLocation", startLocation.value);
    const day = mydate.value.getDate();
    const month = mydate.value.getMonth() + 1;
    const year = mydate.value.getFullYear();
    formData.append("startDate",`${year}-${month}-${day}`);
    formData.append("logo", file);
    const config = {
        headers: {
            'Access-Control-Allow-Origin' : '*',
            'Access-Control-Allow-Methods':'GET'
        }
    }
    axios.post('http://146.190.192.127:8080/v1/campaign/upload', formData, config).then(response => {
              console.log(response.data.campaignId);
          })  
          .catch(errors => console.log(errors));

    // good link https://stackoverflow.com/questions/41878838/how-do-i-set-multipart-in-axios-with-react. No need to set content-type 
    // multipart as based on formData axios would do
};
</script>
