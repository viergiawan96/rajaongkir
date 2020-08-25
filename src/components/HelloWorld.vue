<template>
  <div class="w-9/12 mx-auto my-10">
      <div class="flex flex-rows">
        <div class="hello bg-gray-200 flex-initial w-3/12 border border-solid border-gray-400">
          <div class="bg-white my-2 mx-2 border border-solid border-gray-400">
            <div class="flex flex-col py-5 mx-2">
                <div class="text-sm border-b border-solid py-2">
                  <h1>PERIKSA ONGKOS KIRIM</h1>
                </div>
                
                <div class="text-center flex flex-row mt-4 text-sm">
                  <h1 class="border-t border-r border-l border-solid px-2 py-2 cursor-pointer">Domestik</h1>
                  <h1 class="px-2 py-2 text-teal-500 cursor-not-allowed">International</h1>
                </div>

                <div class="border-t border-solid mt-1 text-base py-5">
                    <div class="">
                        <label for="">kota asal:</label>
                        <v-select @input="getSelectedFroms" :options="City" label="city_name" index="id"></v-select>
                    </div>

                    <div class="my-4 cursor-pointer">
                        <label for="">kota tujuan:</label>
                        <v-select @input="getSelectedTos" :options="City" label="city_name" index="id"></v-select>
                    </div>

                    <div class="my-4 cursor-pointer">
                        <label for="">Berat kiriman(gram):</label>
                        <input v-model="weight" class="border border-solid border-gray-500 py-1 px-1 w-8/12" type="number" name="" id="" placeholder="Ketik angka saja">
                    </div>

                    <button @click="getCost()" class="border border-solid px-4 py-2 mt-4 text-sm bg-teal-500 text-white hover:bg-teal-600" type="submit">Periksa Ongkir</button>
                </div>
            </div>
          </div>
        </div>
        <div class="flex-auto">
          <Container @unFlag="unFlag" :dataCost="getDataCost" :City="City" :flagJne="flagJne" :flagTiki="flagTiki" :flagPos="flagPos"/>
        </div>
      </div>
  </div>
</template>

<script>
import Container from './Container.vue';
import axios from 'axios';
import 'vue-select/dist/vue-select.css';

export default {
  name: 'HelloWorld',
  components:{
    Container
  },
  data(){
    return{
      City : [],
      selectedFrom : "",
      selectedTo : "",
      weight : '',
      courier : ['jne','tiki','pos'],
      getDataCost : [],
      flagJne : false,
      flagTiki : false,
      flagPos : false,
      flagActive : false
      access_token : '<token>'
    }
  },
   methods:{
      unFlag(value) {
        const data = this.getDataCost;

      },
     getSelectedFroms(value){
       this.selectedFrom = value.city_id; 
     },
     
     getSelectedTos(value){
       this.selectedTo = value.city_id;
     },
     getFlag(value){
       const items1 = value.find(el => el.code === 'jne');
       const items2 = value.find(el => el.code === 'tiki');
       const items3 = value.find(el => el.code === 'pos');

        if(items1) {
          this.flagJne = true;
        } 
        if(items2) {
          this.flagTiki = true;
        }
        if(items3) {
          this.flagPos = true;
        }

     },
     async GetCity(){
      const URL ='https://rajaongkir.satrioyudho.com/city';

      await axios.get(URL, {
              headers: {
                'Authorization': `bearer ${this.access_token}`
              }
            })
            .then(res => {
                this.City = res.data.data;
            })
            .catch(err => {
                console.log(err);
            })
      },

      getCost() {
        if(!this.selectedFrom){
              alert('kota asal Kosong');
        }else if(!this.selectedTo) {
              alert('kota tujuan Kosong');
        }else if(!this.weight) {
              alert('Berat Kiriman Kosong');
        }else {
            const URL ='https://rajaongkir.satrioyudho.com/getCost';

            let params = {
                'origin' : this.selectedFrom,
                'destination' : this.selectedTo,
                'weight' : this.weight,
                'courier' : this.courier
              };

            axios.post(URL, params,{
              headers : {
                'Authorization' : `bearer ${this.access_token}`
              }
            })
            .then( res => {
              this.getDataCost = res.data.data;
              this.getFlag(res.data.data.results);
            })
            .catch( err => {
              console.log(err);
            });
        }
  
      }
  },

  created(){

    this.GetCity();

  }
}
</script>


<style scoped>
.hello{
  max-height: 29.7rem;
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

</style>
