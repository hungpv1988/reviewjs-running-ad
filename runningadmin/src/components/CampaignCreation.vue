<template>
    <div class="container" id="camp-panel">
        <form>
            <div class="row">
                <div class="col-md-2"> <label>Name</label> </div>
                <div class="col-md-4"> <input class="form-control" type="text"/> </div>
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
        <!-- <div class="row" v-for="item in items" v-bind:key="item.name">
            <div>
                 <div class="col-md-3 col-lg-3">
                            aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
                </div>
                <div class="col-md-9 col-lg-9">
                                {{item.age}}
                </div>
            </div>
        </div>     -->
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
    formData.append("campaignName","testname");
    formData.append("description","testdesc");
    formData.append("startLocation","teststartlocation");
    const day = mydate.value.getDate();
    const month = mydate.value.getMonth() + 1;
    const year = mydate.value.getFullYear();
    formData.append("startDate",`${year}/${month}/${day}`);
    formData.append("logo", file);

    axios.post('http://146.190.192.127:8080/v1/campaign/upload', formData).then(response => {
              console.log(response.data);
          })  
          .catch(errors => console.log(errors));

//     axios.get('https://localhost:44301/Running?bip=4' , {
//     headers:{
//       'X-Requested-With': 'XMLHttpRequest',
//       'Access-Control-Allow-Origin' : '*',
//       'Access-Control-Allow-Methods':'GET'
//     }
//  }).then(response=>{
//     console.log("test");
//  });
       
};
</script>
