<template>
  <div>
    <h2>Kies je vakantie</h2>
    <div class="row">
      <div class="col-6">
        <div class="btn-group" role="group" aria-label="Basic example">
          <button class="btn btn-success" v-bind:disabled="isBackDisabled" v-on:click="backClick()">« Vorige</button>
          <button class="btn btn-success" v-bind:disabled="true">{{ chosenCountry.id + 1 }}</button>
          <button class="btn btn-success" v-bind:disabled="isNextDisabled" v-on:click="nextClick()">Volgende »</button>
        </div>
        <br><br>
        <button class="btn btn-primary" v-on:click="toggleDetails()">{{ showChosenCountryButtonText }}</button>
        <!--<ul class="list-group">
          <li class="list-group-item" v-on:click="setCountry(country);" v-for="country in countryData.countries" v-bind:key="country.id" v-bind:title="country.details">
          {{ country.id + 1 }}. {{ country.capital }} in {{ country.name }}
          </li>
        </ul>-->
      </div>
      
      <div class="col-6">
        <ul class="list-group">
          <li class="list-group-item">
            <h3>{{ chosenCountry.name }}</h3>
          </li>
          <li class="list-group-item">
            <h4>Hoofdstad: {{ chosenCountry.capital }}</h4>
          </li>
          <li class="list-group-item" v-show="showChosenCountry">
            Reizen naar {{ chosenCountry.name }} kost je €{{ chosenCountry.cost }}. <!--<p><span class="badge badge-primary"> {{ chosenCountry.cost }} </span></p>-->
          </li>
          
          <li class="list-group-item" v-show="showChosenCountry">
            <img v-bind:src="getImgUrl(chosenCountry.img)" v-bind:alt="chosenCountry.img" class="img-fluid" />
          </li>
          <li class="list-group-item" v-show="showChosenCountry">
            {{ chosenCountry.details }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import countryData from '@/data/countryData'
export default {
  name: 'VacationPicker',
  data() {
    return {
      countryData,
      chosenCountry: countryData.countries[0],
      isBackDisabled: true,
      isNextDisabled: false,
      showChosenCountry: true,
      showChosenCountryButtonText: "Verberg details"
    }
  },
  methods: {
    backClick() {
      this.setCountry(this.countryData.countries[this.chosenCountry.id - 1]);
      if(this.chosenCountry.id < 1) {
        this.isBackDisabled = true;
      }
      this.isNextDisabled = false;
    },
    nextClick() {
      this.setCountry(this.countryData.countries[this.chosenCountry.id + 1]);
      this.isBackDisabled = false;
      if((this.chosenCountry.id + 1) >= this.countryData.countries.length) {
        this.isNextDisabled = true;
      }
    },
    toggleDetails() {
      this.showChosenCountry = !this.showChosenCountry;
      if(this.showChosenCountry) {
        this.showChosenCountryButtonText = "Verberg details";
      } else {
        this.showChosenCountryButtonText = "Toon details";
      }
    },
    setCountry(country) {
      this.chosenCountry = country;
    },
    getImgUrl(img) {
      console.log(img);
      return require('../assets/countries/' + img);
    }
  }
}
</script>