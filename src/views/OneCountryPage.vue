<template>
  <main class="oneCountry">
    <div class="main">
      <div class="back-button">
        <router-link to="/">
          <button class="button is-rounded">Back</button>
        </router-link>
      </div>
    </div>
    <div class="country" v-if="json && json[0]">
      <div class="one-flag-wrap" v-if="json[0].flags && json[0].flags.svg">
        <img :src="json[0].flags.svg" alt="img" />
      </div>
      <div class="statistics-wrap">
        <div class="country-name" v-if="json[0].name && json[0].name.common">
          <h1 class="has-text-weight-bold">{{ json[0].name.common }}</h1>
        </div>
        <div class="information-wrap">
          <div class="data-wrap">
            <p v-if="json[0].altSpellings">
              <strong>Native Name:</strong> {{ json[0].altSpellings[1] }}
            </p>
            <p v-if="json[0].population">
              <strong>Population:</strong>
              {{ json[0].population.toLocaleString("en-GB") }}
            </p>
            <p v-if="json[0].region">
              <strong>Region:</strong> {{ json[0].region }}
            </p>
            <p>
              <strong v-if="json[0].subregion">Sub Region</strong>
              {{ json[0].subregion }}
            </p>
            <p v-if="json[0].capital">
              <strong>Capital:</strong> {{ json[0].capital[0] }}
            </p>
          </div>
          <div class="data-wrap">
            <div class="string-wrap" v-if="this.money">
              <p><strong>Currencies: </strong></p>
              <!-- <p v-for="currency in json[0].currencies" :key="currency.name"> {{currency.name}}</p> -->
              <p>{{ this.money }}</p>
            </div>
            <div class="string-wrap" v-if="this.names">
              <p><strong>Languages: </strong></p>
              <!-- <p v-for="language in json[0].languages" :key="language.name"> {{language.name}}</p> -->
              <p>{{ this.names }}</p>
            </div>
          </div>
        </div>
        <div class="string-wrap" v-if="borders">
          <p><strong class="border-string">Border Countries: </strong></p>
          <p>{{ borders }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "OneCountryPage",
  data() {
    return {
      json: "",
      error: "",
      borders: "",
      names: "",
      money: "",
    };
  },
  methods: {
    getCountry() {
      const name = this.$route.params.name;

      fetch(`https://restcountries.com/v3.1/name/${name}`)
        .then((res) => {
          return res.json();
        })
        .then((result) => {
          this.json = result;
          this.getBorderCountries(this.json[0].borders);
          this.getCurrencies();
          this.getLanguages();
        })
        .catch((err) => {
          this.error = err;
        });
    },
    getBorderCountries(borders) {
      const codes = borders.join(",");
      if (codes) {
        fetch(`https://restcountries.com/v3.1/alpha?codes=${codes}`)
          .then((res) => {
            return res.json();
          })
          .then((result) => {
            let borderCountries = [];
            for (let i = 0; i < result.length; i++) {
              borderCountries.push(result[i].name.common);
            }
            this.borders = borderCountries?.join(", ");
          })
          .catch((err) => {
            this.error = err;
          });
      }
    },
    getCurrencies() {
      let currencies = this.json[0].currencies;

      let moneyData = [];
      Object.keys(currencies).forEach(function(key) {
        moneyData.push(currencies[key]["name"]);
      });

      this.money = moneyData?.join(", ");
    },
    getLanguages() {
      let languages = this.json[0].languages;

      let languagesData = [];
      Object.keys(languages).forEach(function(key) {
        languagesData.push(languages[key]);
      });

      this.names = languagesData?.join(", ");
    },
  },
  mounted() {
    this.getCountry();
  },
};
</script>

<style>
.country {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 0 2 rem;
}
.one-flag-wrap img {
  height: 210px;
  max-width: 80%;
  box-shadow: 0 0.5em 1em -0.125em rgba(10, 10, 10, 0.1),
    0 0 0 1px rgba(10, 10, 10, 0.02);
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
  .one-flag-wrap img {
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
