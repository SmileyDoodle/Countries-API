<template>
  <main class="oneCountry">
    <div class="main">
        <div class="back-button">
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
            <p><strong>Population:</strong> {{json[0].population.toLocaleString("en")}}</p>
            <p><strong>Region:</strong> {{json[0].region}}</p>
            <p><strong>Sub Region</strong> {{json[0].subregion}}</p>
            <p><strong>Capital:</strong> {{json[0].capital}}</p>
          </div>
          <div class="data-wrap">
            <p><strong>Top Level Domain:</strong> {{json[0].topLevelDomain[0]}}</p>
            <div class="string-wrap">
              <p><strong>Currencies: </strong></p>
              <!-- <p v-for="currency in json[0].currencies" :key="currency.name"> {{currency.name}}</p> -->
              <p> {{this.money}} </p>
            </div>
            <div class="string-wrap">
              <p><strong>Languages: </strong></p>
              <!-- <p v-for="language in json[0].languages" :key="language.name"> {{language.name}}</p> -->
              <p> {{this.names}} </p>
            </div>
          </div>
        </div>
        <div class="string-wrap">
          <p><strong class="border-string">Border Countries: </strong></p>
          <p> {{ borders }} </p>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
    name: 'OneCountryPage',
    data() {
      return {
        json: "",
        error: "",
        borders: "",
        names: "",
        money: ""
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
                  this.getBorderCountries(this.json[0].borders);
                  this.getCurrencies();
                  this.getLanguages();
              }).catch((err) => {
                  this.error = err;
              })
      },
      getBorderCountries(borders) {

        const codes = borders.join(';')
        
        fetch(`https://restcountries.eu/rest/v2/alpha?codes=${codes}`)
              .then(res => {
                  return res.json();
              }).then(result => {

                  let borderCountries = [];
                  for (let i = 0; i< result.length; i++) {
                    borderCountries.push(result[i].name);
                  }
                  console.log('borders ', borderCountries)
                  this.borders =  borderCountries.join(', ');

              }).catch((err) => {
                  this.error = err;
              })
      },
      getCurrencies() {
        let currencies = this.json[0].currencies;

        this.money = currencies.map(function(item) {
          return item['name'];
        });
        console.log("money", this.money)
        this.money = this.money.join(', ');
      },
      getLanguages() {
        let languages = this.json[0].languages;

        this.names = languages.map(function(item) {
          return item['name'];
        });
        console.log("names", this.names)
        this.names = this.names.join(', ');
      }
    },
    mounted() {
        this.getCountry();
    }
}
</script>

<style>
.country {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 0 2 rem;
}
.one-flag-wrap img{
  height: 210px;
  max-width: 80%;
  box-shadow: 0 0.5em 1em -0.125em rgba(10,10,10,.1), 0 0 0 1px rgba(10,10,10,.02);
  /* box-shadow: 0px 0px 19px 4px #cde2e6; */
}
.statistics-wrap {
  width: 75%;
  margin-top: 2rem;
}
.country-name h1 {
  font-size: 1.5rem;
  margin-bottom: 1.5rem;
}
.information-wrap {
  text-align: left;
  margin: 1.5rem 0;
}
.data-wrap {
  width: 100%;
}
.data-wrap p {
  font-size: 0.9rem;
  padding: 0.2rem 0;
}
.string-wrap {
  display: flex;
}
.string-wrap p:first-child {
  margin-right: 0.3rem;
}
.string-wrap:last-child p {
  text-align: left;
  font-size: 0.9rem;
}
.border-string {
  display: block;
  width: 120px;
}
.button {
  color: var(--font-color);
}

@media only screen and (min-width: 1024px) {
  .country {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
  padding: 4rem 0;
  }
  .one-flag-wrap img{
    height: 300px;
    max-width: 500px;
  }
  .statistics-wrap {
    width: 40%;
    margin-top: 0;
  }
  .country-name h1 {
    font-size: 2rem;
    text-align: left;
    margin-bottom: 0;
  }
  .information-wrap {
    display: flex;
  }
  .data-wrap {
    width: 50%;
  }
  .border-string {
    width: 121px;
  }
}
</style>