<template>
    <div>
        <div class="main">
            <div class="input-wrap">
                <input 
                    type="text" 
                    class="input is-rounded"
                    placeholder="Search for a country..." 
                    v-model="query"
                    @keyup.enter="getCountry()"
                />
                <div class="hidden-wrap" v-show="isSeen">
                    <h1 class="has-text-weight-bold"> Can't find the country. Try again!</h1>
                </div>
            </div>
            <div id="overlay" v-if="showDrop" @click="showDrop = !showDrop"></div>
            <div class="dropdown" @click="showDrop = !showDrop" :class="{ 'is-active': showDrop }">
                <div class="dropdown-trigger">
                        <button class="button is-rounded" aria-haspopup="true" aria-controls="dropdown-menu">
                        <span>Filter by Region</span>
                        <span class="icon is-small">
                            <i class="fas fa-angle-down" aria-hidden="true"></i>
                        </span>
                        </button>
                    </div>
                    <div class="dropdown-menu" id="dropdown-menu" role="menu">
                        <div class="dropdown-content">
                            <a href="#" class="dropdown-item" @click="getRegion('africa')" :class="{'is-active': selectedValue === 'africa'}">
                                Africa
                            </a>
                            <a href="#" class="dropdown-item" @click="getRegion('americas')" :class="{'is-active': selectedValue === 'americas'}">
                                America
                            </a>
                            <a href="#" class="dropdown-item" @click="getRegion('asia')" :class="{'is-active': selectedValue === 'asia'}">
                                Asia
                            </a>
                            <a href="#" class="dropdown-item" @click="getRegion('europe')" :class="{'is-active': selectedValue === 'europe'}">
                                Europe
                            </a>
                            <a href="#" class="dropdown-item" @click="getRegion('oceania')" :class="{'is-active': selectedValue === 'oceania'}">
                                Oceania
                            </a>
                            <hr class="dropdown-divider">
                            <a href="#" class="dropdown-item" @click="getAllCountries()" :class="{'is-active': selectedValue === 'all' || selectedValue === ''}">
                                All
                            </a>
                        </div>
                    </div>
                </div>
        </div>
        <div>
           <Countries :region="region"></Countries>
        </div>
    </div>
</template>

<script>
import Countries from '@/components/Countries.vue'

export default {
    name: 'Main',
    components: {
        Countries,
    },
    data() {
      return {
        query: "",
        json: "",
        error: "",
        showDrop: false,
        isSeen: false,
        region: "",
        selectedValue: ""
      }
    },
    methods: {
      getCountry() {
            
            fetch(`https://restcountries.eu/rest/v2/name/${this.query}`)
              .then(res => {
                  return res.json();
              }).then(result => {

                  if (result.status) {
                      this.isSeen = true;
                  } else {
                      this.$router.push({ path: `country/${this.query}` });
                  }
              })
      },
      getRegion(region) {
        this.selectedValue = region;
        fetch(`https://restcountries.eu/rest/v2/region/${region}`)
            .then(res => {
                return res.json();
            }).then(result => {
                this.region = result;
                // console.log(this.region);
            }).catch((err) => {
                this.error = err;
            })
      },
      getAllCountries() {
        this.selectedValue = "all";
        fetch(`https://restcountries.eu/rest/v2/all`)
            .then(res => {
                return res.json();
            }).then(result => {
                this.region = result;
                // console.log("json", this.json);
            }).catch((err) => {
                this.error = err;
            })
      },
    }
}
</script>

<style>
#overlay {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    z-index: 0;
}

.input {
    width: 250px;
}
.input-wrap {
    display: flex;
    width: 551px;
    align-items: center;
}
.hidden-wrap {
    width: 300px;
    font-size: 1.1rem;
    color: red;
}
.main {
    widows: 100%;
    display: flex;
    height: 100px;
    justify-content: space-between;
    align-items: center;
    padding: 0 3rem;
}
.dropdown {
    z-index: 1;
}
.dropdown-content a {
    text-align: left;
}
</style>