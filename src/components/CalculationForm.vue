<template>
  <div class="wrapper">
    <div class="calculatingForm">
      <div class="title"><span>Calculate the cost of deliver</span></div>
      <form @submit="calculate">
        <div class="row">
          <i class="fas fa-user"></i>
          <input type="text" placeholder="Weight" v-model="weight" required>
        </div>
        <div class="row">
          <i class="fas fa-lock"></i>
          <select v-model="company">
            <option v-for="deliveryService in deliveryServices" :key="deliveryService.id" :value="deliveryService.id">{{deliveryService.id + "   " + deliveryService.name}}</option>
          </select>
        </div>
        <div class="row button">
          <input type="submit" value="Calculate">
        </div>
        <div v-if="cost > 0" class="row">
          <i class="fas fa-user"></i>
          <input type="text" v-model="costString" readonly>
        </div>
      </form>
    </div>
  </div>
</template>


<script setup lang="ts">
import {computed, onMounted, ref} from "vue";
import axios from "axios";

const apiPath:string = process.env.VUE_APP_ROOT_API;
const cost:object = ref(0);
const weight:object = ref();
const company:object = ref();
const deliveryServices:object = ref([]);
const costString:object = computed(() => cost.value + '€');

onMounted(() => {
  axios.get(apiPath + '/get-delivery-services').then(response => {
    deliveryServices.value = response.data;
  });
});
const calculate = (event):void => {
  event.preventDefault();
  axios.post(apiPath + '/calculate-delivery-cost', JSON.stringify({
    id: company.value,
    weight: weight.value
  })).then(response => {
    cost.value = response.data.cost;
  });
}

</script>



<style scoped lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins',sans-serif;
}
body{
  background: #1abc9c;
  overflow: hidden;
}
::selection{
  background: rgba(26,188,156,0.3);
}
.container{
  max-width: 440px;
  padding: 0 20px;
  margin: 170px auto;
}
.wrapper{
  margin: auto;
  margin-top: 125px;
  width: 45%;
  background: #fff;
  border-radius: 5px;
  box-shadow: 0px 4px 10px 1px rgba(0,0,0,0.1);
}
.wrapper .title{
  height: 90px;
  background: #16a085;
  border-radius: 5px 5px 0 0;
  color: #fff;
  font-size: 30px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
}
.wrapper form{
  padding: 30px 25px 25px 25px;
}
.wrapper form .row{
  height: 45px;
  margin-bottom: 15px;
  position: relative;
}
.wrapper form .row input{
  height: 100%;
  width: 100%;
  outline: none;
  padding-left: 60px;
  border-radius: 5px;
  border: 1px solid lightgrey;
  font-size: 16px;
  transition: all 0.3s ease;
}
.wrapper form .row select{
  height: 100%;
  width: 100%;
  outline: none;
  padding-left: 60px;
  border-radius: 5px;
  border: 1px solid lightgrey;
  font-size: 16px;
  transition: all 0.3s ease;
}
form .row input:focus{
  border-color: #16a085;
  box-shadow: inset 0px 0px 2px 2px rgba(26,188,156,0.25);
}
form .row input::placeholder{
  color: #999;
}
.wrapper form .row i{
  position: absolute;
  width: 47px;
  height: 100%;
  color: #fff;
  font-size: 18px;
  background: #16a085;
  border: 1px solid #16a085;
  border-radius: 5px 0 0 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.wrapper form .pass{
  margin: -8px 0 20px 0;
}
.wrapper form .pass a{
  color: #16a085;
  font-size: 17px;
  text-decoration: none;
}
.wrapper form .pass a:hover{
  text-decoration: underline;
}
.wrapper form .button input{
  color: #fff;
  font-size: 20px;
  font-weight: 500;
  padding-left: 0px;
  background: #16a085;
  border: 1px solid #16a085;
  cursor: pointer;
}
form .button input:hover{
  background: #12876f;
}
.wrapper form .signup-link{
  text-align: center;
  margin-top: 20px;
  font-size: 17px;
}
.wrapper form .signup-link a{
  color: #16a085;
  text-decoration: none;
}
form .signup-link a:hover{
  text-decoration: underline;
}
</style>