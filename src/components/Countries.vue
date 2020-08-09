<template>
    <div class="container row">
        <div class="column country-info" style="background-image: linear-gradient(to bottom right, #9ceaef, #9ceaef);">
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
        <div class="column" style=" background-image: linear-gradient(to bottom right, #d3f8e2, #a9def9);">
            <div class="selected-options">
            <p v-if="selectedCountry">Selected Country: {{ selectedCountry }}</p>
            <p v-if="selectedState">Selected State: {{ selectedState }}</p>
            <p v-if="selectedCity">Selected City: {{ selectedCity }}</p>
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
            this.countries = data.slice(0, 10);
        })    
    },
    methods: {
        onCountrySelect(event){
            const state = event.target.value
            const index = this.countries.findIndex((x=>x.name === state))
            const countryState = this.countries[index]['states']
            if (countryState.length === 0){
                this.stateError = !this.stateError 
                this.states = null   
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

.country-info{
    padding: 20px;
}

.form-controls{
    margin: 10px;
}
.count-select{
    width: 20em;
    height: 2em;
    display: block;
    margin-top: 10px;
    font-size: 1em;
}
.count-option{
    display: block;
    margin-left: 20px;
}

.column{
    float: left;
    width: 50%;
    padding: 10px;
    height: 100vh;
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
    width: 30em;
    height: 3em;
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
</style>