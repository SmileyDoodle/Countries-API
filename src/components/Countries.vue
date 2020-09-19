<template>
  <div class="content-wrap">
      <div class="card-wrap" v-for="data in changeRegion" :key="data.id" @click="goOnePage(data.name)">
        <div class="box">
          <div class="img-wrap">
            <img :src="data.flag" alt="img" class="flag-wrap">
          </div>
          <div class="info-wrap">
            <h1><strong> {{data.name}} </strong></h1>
            <p><strong>Population:</strong> {{data.population.toLocaleString("en")}} </p>
            <p><strong>Region:</strong> {{data.region}} </p>
            <p><strong>Capital:</strong> {{data.capital}} </p>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
    name: 'Countries',
    props: ['region'],
    data() {
      return {
        json: "",
        error: ""
      }
    },
    computed: {
      changeRegion: function() {
        if (this.region === "") {
          return this.json;
        } else {
          return this.region;
        }
      }
    },
    methods: {
      getAllCountries(data) {
        fetch(`https://restcountries.eu/rest/v2/all`)
            .then(res => {
                return res.json();
            }).then(result => {
                this.json = result;
                // console.log("json", this.json);
                console.log("data", data)
            }).catch((err) => {
                this.error = err;
            })
      },
      goOnePage(countryName) {
            const myStorage = window.localStorage;
            myStorage.setItem('country', countryName);
            this.$router.push({ path: `country/${countryName}`});
      }
    },
    mounted() {
      this.getAllCountries();
    }
}
</script>

<style>
.content-wrap {
  width: 100%;
  display: flex;
  flex: 1;
  flex-flow: wrap;
}
.card-wrap {
  width: 100%;
  height: 325px;
  display: flex;
  justify-content: center;
  margin: 3rem 0;
}
.box {
  width: 75%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.img-wrap {
  height: 63%;
}
.flag-wrap {
  height: 100%;
  box-shadow: 0 0.5em 1em -0.125em rgba(10,10,10,.1), 0 0 0 1px rgba(10,10,10,.02);
  /* box-shadow: 0px 0px 14px -5px #cde2e6; */
}
.info-wrap {
  text-align: left;
}

.box {
  background-color: var(--secondary-color);
}

@media only screen and (min-width: 1024px) {
  .card-wrap {
    width: 25%;
    height: 275px;
    margin: 1rem 0;
  }
  .box {
    width: 75%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .img-wrap {
    height: 50%;
  }
}
</style>