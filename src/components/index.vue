<template>
  <div class="container-fluid w-100" >
      <div class="row " >
          <div class="card w-100" >

              <div class="card-header bg-white" >
                  <div class="ml-5">
                       <h2>Where in the World?</h2>
                  </div>
              </div>

            <div class="card-body">

                <div id="body-header" class="w-100 mb-4" >

                    <div text-align="left" class="col-lg-4 mb-3" id="searchBar" >
                        <i class="fa fa-search icon" aria-hidden="true"></i> 
                        <input  id="input" v-model="countryName" class="form-control col-lg-2" type="text" placeholder="Search for a country..." >
                    </div>

                    <div class="col-md-2 mb-3">
                       <select v-model="getcountriesByRegion"  class="form-select" name="regions" id="regions"  >
                            <option value="">Todos</option>
                            <option value="Africa">Africa</option>
                            <option value="Americas">Americas</option>
                            <option value="Asia">Asia</option>
                            <option value="Europe">Europe</option>
                            <option value="Oceania">Oceania</option>
                       </select>     
                    </div>

                </div>
             
                <div id="countries">
                    <div class="card mb-4" id="country" v-for="country in searchCountry" v-bind:key="country.alpha2Code"  style="width: 18rem;">
                            <img class="card-img-top" :src="country.flag" alt="Card image cap">
                            <div class="card-body bg-light" >
                                <h3 class="my-3" > {{country.name}} </h3>
                                <h5 class="card-title">Population: {{country.population}}</h5>
                                <h5 class="card-title">Region: {{country.region}}</h5>
                                <h5 class="card-title">Capital: {{country.capital}}</h5>
                            </div>

                    </div>
                </div>
               

                
            </div>

        </div>
    
      </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Index',
    data(){
        return{
            countryList:[],
            getcountriesByRegion:'',
            countriesByRegion:[],
            countriesTotal : [],
            countryName:''
        }
    },
    
    created(){
        axios
        .get('https://restcountries.eu/rest/v2/all')
        .then( response =>{
            this.countryList = response.data
            this.countriesTotal = this.countryList
        })
        .catch( error => console.log( "ERROR" , error) )
    },


    computed:{
        searchCountry: function(){
            if(this.getcountriesByRegion != ''){
            return this.countriesTotal.filter(country => country.region == this.getcountriesByRegion );
            }
            return this.countriesTotal.filter( country => country.name.toLowerCase().includes(this.countryName.toLowerCase()) );
        }
    }

}
</script>

<style  scoped>
#countries{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
}

@media only screen and (min-width: 375px) {
    #country{
    margin-left:80px;
    margin-right:80px;
    }   

}

#body-header{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.card-body{
    background-color: #EEEDED;
}
 #searchBar { 
     margin-bottom: 10px;
     display: flex;
     align-content: flex-start;
     
 } 
    
#searchBar i { 
     position: absolute; 
 } 
   
      
.icon { 
        padding: 10px; 
        min-width: 40px; 
    } 
          
#input { 
        text-align: left; 
        padding-left: 50px;
    } 


</style>