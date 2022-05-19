<template>
  <div class="container">
    <div>
      <h1 class="subtitle text-2xl my-20">
        Exchange Rate
      </h1>
      <div class="inputs">
        <input @input="calculate" type="number" class="amount mx-15 px-5 py-2 shadow-xl" v-model="amount" >
        <input @input="calculate" type="text" class="from mx-15 px-5 py-2 shadow-xl" v-model="from">
        <input @input="calculate" type="text" class="to mx-15 px-5 py-2 shadow-xl" v-model="to">
        <!--<the-mask :mask="['### ### ### ### ### ###']" />-->

      </div>
      <div class="title">
        {{result}}
      </div>
    </div>
  </div>
</template>

<script>
import {TheMask} from 'vue-the-mask'

export default {
  components: {TheMask},
  data(){
    return {
      amount:1,
      from:"USD",
      to:"TRY",
      result:0,
      apiKey:"lvkLuUPOAjlx8q06PdimxFnmT5QstBwi"
    }
  },
  methods:{
    calculate(){
      var myHeaders = new Headers();
      myHeaders.append("apikey", this.apiKey);

      var requestOptions = {
        method: 'GET',
        redirect: 'follow',
        headers: myHeaders
      };
      if(this.amount>0){
        fetch("https://api.apilayer.com/exchangerates_data/convert?to="+this.to+"&from="+this.from+"&amount="+this.amount, requestOptions)
        .then(response => response.text())
        .then(result => {
          console.log(JSON.parse(result))
          this.result=JSON.parse(result).result
        }
        )
        .catch(error => console.log('error', error));
      }else{
        this.result=0
      }
      
      
    }
  },
  created(){
    this.calculate()
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
</style>
