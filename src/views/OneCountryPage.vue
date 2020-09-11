<template>
  <div class="oneCountry">
    <Header></Header>
    <div class="main">
        <div>
          <button>Back</button>
        </div>
      </div>
    <div class="country">
      <div class="one-flag-wrap">
        <img :src="json[0].flag" alt="img">
      </div>
      <div>
        <div>
          <h1>{{json[0].name}}</h1>
        </div>
        <div>
          <div>
            <p><strong>Native Name:</strong> {{json[0].altSpellings[1]}}</p>
            <p><strong>Population:</strong> {{json[0].population}}</p>
            <p><strong>Region:</strong> {{json[0].region}}</p>
            <p><strong>Sub Region</strong> {{json[0].subregion}}</p>
            <p><strong>Capital:</strong> {{json[0].capital}}</p>
          </div>
          <div>
            <p><strong>Top Level Domain:</strong> {{json[0].topLevelDomain[0]}}</p>
            <div>
              <strong>Currencies:</strong> 
              <p v-for="currency in json[0].currencies" :key="currency"> {{currency.name}}</p>
            </div>
            <div>
              <strong>Languages:</strong> 
              <p v-for="language in json[0].languages" :key="language"> {{language.name}}</p>
            </div>
          </div>
        </div>
        <div>
          <strong>Border Countries:</strong>
          <p v-for="border in json[0].borders" :key="border"> {{border}}</p>
        </div>
      </div>
    </div>
    <Footer></Footer>
  </div>
</template>

<script>
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'

export default {
    name: 'OneCountryPage',
    components: {
      Header,
      Footer
    },
    data() {
      return {
        json: "",
        error: ""
      }
    },
    methods: {
      getCountry() {
          let myCountry = window.localStorage.getItem('country');

          fetch(`https://restcountries.eu/rest/v2/name/${myCountry}`)
              .then(res => {
                  return res.json();
              }).then(result => {
                  this.json = result;
                  console.log(this.json)
              }).catch((err) => {
                  this.error = err;
              })
      }  
    },
    mounted() {
      if (this.myCountry !== "") {
        this.getCountry();
      }
    }
}
</script>

<style>
/* .oneCountry {
  height: 100%;
}
.country {
  height: 85%;
} */
</style>