<template>
    <div class="container row">
        <div class="title"  style="text-align: center">
            <h1>{{title}}</h1>
            <p style="margin-top: 30px; font-weight: normal;">Select a country below to reveal its states and the cities in the state selected</p>
        </div> 
        <div class="country-info">
            <div class="column column-1" style="background-color: white">
                <form action="">
                        <div class="form-controls">
                            <label for="countries" class="select-label">Select Country</label>
                            <select name="countries" id="countries" v-model="selectedCountry" @change="onCountrySelect($event)" class="count-select">
                                <option :value=country.name v-for="country in countries" :key="country.id" class="count-option">{{ country.name }}</option>
                            </select>
                        </div>
                        <div class="form-controls">
                            <label for="state">Select State</label>
                            <select name="states" id="states" v-model="selectedState" @change="onStateSelect($event)" class="count-select">
                                <option :value="state.name" v-for="state in states" :key="state.id" class="count-option">{{ state.name}}</option>
                            </select>
                            <div v-if="stateError" :class="{empty: stateError}">
                                <p>This country has no states. Kindly select another country</p>
                            </div>
                        </div>
                        <div class="form-controls">
                            <label for="cities">Select City</label>
                            <select name="cities" id="cities" v-model="selectedCity" class="count-select">
                                <option :value="city.name" v-for="city in cities" :key="city.id" class="count-option">{{ city.name}}</option>
                            </select>
                            <div v-if="cityError" :class="{empty: cityError}">
                                <p>This state has no cities. Kindly select another state or country</p>
                            </div>
                        </div>      
                </form>
            </div>
            <div class="column column-2" style=" background-color: white;">
                <div class="selected-options">
                <p v-if="selectedCountry" style="margin-bottom: 100px">Selected Country: <span style="font-weight: normal"> {{ selectedCountry }} </span></p>
                <p v-if="selectedState" style="margin-bottom: 100px">Selected State: <span style="font-weight: normal"> {{ selectedState }} </span></p>
                <p v-if="selectedCity" style="margin-bottom: 100px">Selected City: <span style="font-weight: normal"> {{ selectedCity }} </span></p>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            selectedCountry: "",
            selectedState: "",
            selectedCity: "",
            title: "WeJapa Countries",
            stateError: false,
            cityError: false,
            countries: [],
            states: [],
            cities: []
        }
    },
    created(){
        this.$http.get('https://raw.githubusercontent.com/dr5hn/countries-states-cities-database/master/countries%2Bstates%2Bcities.json').then(
            response=>{
                return response.json()
            }
        ).then(data=>{
            this.countries = data;
        })    
    },
    methods: {
        onCountrySelect(event){
            const state = event.target.value
            const index = this.countries.findIndex((x=>x.name === state))
            const countryState = this.countries[index]['states']
            if (countryState.length === 0){
                this.stateError = !this.stateError 
                this.states = ""   
                this.cities = ""
            } else{
                this.states = countryState
                this.stateError = false
            }
        },
        onStateSelect(event){
            const state = event.target.value
            const index = this.states.findIndex((x=>x.name === state))
            const city = this.states[index]['cities']
            if (city.length === 0){
                this.cityError = !this.cityError
            }else{
                this.cities = city
                this.cityError = false
            }

        }
    }
}
</script>

<style scoped>

.title{
    margin-top: 20px;
}

.select-label{
    margin-bottom: 30px;
}

.country-info{
    padding: 50px;
}

.form-controls{
    margin: 10px;
}
.count-select{
    background: none repeat scroll 0 0 #FFFFFF;
    border: 1px solid #E5E5E5;
    border-radius: 5px 5px 5px 5px;
    box-shadow: 0 0 10px #E8E8E8 inset;
    height: 3em;
    margin: 20px 0;
    font-size: 1em;
    display: block;
    padding: 10px;
    width: 12em;
}
.count-option{
    display: block;
    margin-left: 20px;
    
}

.column-1{
    float: left;
    width: 50%;
    padding-left: 20% !important;
    height: 75vh;
    margin: auto
}

.column-2{
    padding-left: 5% !important;
}

.row:after {
  content: "";
  display: table;
  clear: both;
}

.empty{
    border: 1px solid;
    background-color: rgb(226, 64, 64);
    color: white;
    width: 15em;
    height: 6em;
    padding: 20px;
    border-radius: 6px;
    margin-top: 10px;
}

.selected-options{
    margin-top: 10px;
}

.selected-options p {
    margin-bottom: 10px;
}

@media screen and (max-width: 1024px) {
  .column-1 {
    float: left;
    width: 50%;
    padding-left: 12% !important;
    height: 100vh;
}
.column-2 {
    float: left;
    width: 50%;
    padding-left: 10% !important;
    height: 100vh;
}
}
@media screen and (max-width: 768px) {
  .column-2 {
    float: left;
    width: 50%;
    padding-left: 8% !important;
    height: 100vh;
}
.column-2 {
    float: left;
    width: 50%;
    padding-left: 10% !important;
    height: 100vh;
}
}

@media screen and (max-width: 575px) {
  .column-1 {
    float: left;
    width: 50%;
    padding-left: 8% !important;
    height: 100vh;
}
.column-2 {
    position: absolute;
    margin-top: 120% !important;
    margin-bottom: 2em;
    padding-left: 8% !important;
    left: 50%;
    transform: translateX(-50%) !important;
    width: 100% !important;
    height: auto;
}
}

@media screen and (max-width: 414px) {
  .column-1, .column-2 {
    padding-left: 8% !important;
}
} 

</style>