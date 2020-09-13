<template>
  <div class="oneCountry">
    <Header></Header>
    <div class="main">
        <div>
          <router-link to="/">
            <button class="button is-rounded">Back</button>
          </router-link>
        </div>
      </div>
    <div class="country" v-if="json[0]">
      <div class="one-flag-wrap">
        <img :src="json[0].flag" alt="img">
      </div>
      <div class="statistics-wrap">
        <div class="country-name">
          <h1 class="has-text-weight-bold">{{json[0].name}}</h1>
        </div>
        <div class="information-wrap">
          <div class="data-wrap">
            <p><strong>Native Name:</strong> {{json[0].altSpellings[1]}}</p>
            <p><strong>Population:</strong> {{json[0].population}}</p>
            <p><strong>Region:</strong> {{json[0].region}}</p>
            <p><strong>Sub Region</strong> {{json[0].subregion}}</p>
            <p><strong>Capital:</strong> {{json[0].capital}}</p>
          </div>
          <div class="data-wrap">
            <p><strong>Top Level Domain:</strong> {{json[0].topLevelDomain[0]}}</p>
            <div class="string-wrap">
              <p><strong>Currencies: </strong></p>
              <p v-for="currency in json[0].currencies" :key="currency.name"> {{currency.name}}</p>
            </div>
            <div class="string-wrap">
              <p><strong>Languages: </strong></p>
              <p v-for="language in json[0].languages" :key="language.name"> {{language.name}}</p>
            </div>
          </div>
        </div>
        <div class="string-wrap">
          <p><strong>Border Countries:</strong></p>
          <p v-for="border in json[0].borders" :key="border"> {{border}} </p>
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
          const name = this.$route.params.name;
          
          fetch(`https://restcountries.eu/rest/v2/name/${name}`)
              .then(res => {
                  return res.json();
              }).then(result => {
                  this.json = result;
                  console.log(this.json)
              }).catch((err) => {
                  this.error = err;
              })
      },
    },
    mounted() {
        this.getCountry();
    }
}
</script>

<style>
.country {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: 4rem 0;
}
.one-flag-wrap img{
  height: 300px;
}
.statistics-wrap {
  width: 40%;
}
.country-name h1 {
  font-size: 2rem;
  text-align: left;
}
.information-wrap {
  display: flex;
  text-align: left;
  margin: 1.5rem 0;
}
.data-wrap {
  width: 50%;
}
.data-wrap p {
  font-size: 0.9rem;
  padding: 0.2rem 0;
}
.string-wrap {
  display: flex;
} 
</style>