<template>
  <div class="content-wrap">
    <div
      class="card-wrap"
      v-for="data in changeRegion"
      :key="data.id"
      @click="goOnePage(data.name.common)"
    >
      <div class="box">
        <div class="img-wrap" v-if="data.flags && data.flags.svg">
          <img :src="data.flags.svg" alt="img" class="flag-wrap" />
        </div>
        <div class="info-wrap">
          <h1 v-if="data.name && data.name.official">
            <strong> {{ data.name.official }} </strong>
          </h1>
          <p v-if="data.population">
            <strong>Population:</strong>
            {{ data.population.toLocaleString("en-GB") }}
          </p>
          <p v-if="data.region"><strong>Region:</strong> {{ data.region }}</p>
          <p v-if="data.capital">
            <strong>Capital:</strong> {{ data.capital[0] }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Countries",
  props: ["region"],
  data() {
    return {
      json: "",
      error: "",
    };
  },
  computed: {
    changeRegion: function() {
      if (this.region === "") {
        return this.json;
      } else {
        return this.region;
      }
    },
  },
  methods: {
    getAllCountries() {
      fetch(`https://restcountries.com/v3.1/all`)
        .then((res) => {
          return res.json();
        })
        .then((result) => {
          this.json = result;
        })
        .catch((err) => {
          this.error = err;
        });
    },
    goOnePage(countryName) {
      const myStorage = window.localStorage;
      myStorage.setItem("country", countryName);
      this.$router.push({ path: `name/${countryName}` });
    },
  },
  mounted() {
    this.getAllCountries();
  },
};
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
.box:hover {
  cursor: pointer;
}
.img-wrap {
  height: 63%;
}
.flag-wrap {
  height: 100%;
  box-shadow: 0 0.5em 1em -0.125em rgba(10, 10, 10, 0.1),
    0 0 0 1px rgba(10, 10, 10, 0.02);
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
