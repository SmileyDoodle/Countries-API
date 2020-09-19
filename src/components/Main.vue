<template>
    <main>
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
    </main>
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

.main {
    width: 100%;
    display: flex;
    flex-direction: column;
    height: 100px;
    justify-content: space-between;
    align-items: center;
}
.input-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 60px;
    width: 65%;
}
.input {
    width: 100%;
    background-color: var(--secondary-color);
    border-color: var(--border-color);
    color: var(--input-text);
}
.hidden-wrap {
    width: 100%;
    font-size: 0.7rem;
    color: red;
}
.dropdown {
    z-index: 1;
    width: 65%;
    justify-content: center;
}
.dropdown-menu {
    left: unset;
}
.dropdown-content {
    text-align: left;
    background-color: var(--secondary-color);
}

.dropdown-item {
    color: var(--font-color);
}
.button {
    background-color: var(--secondary-color);
    border-color: var(--border-color);
}

@media only screen and (min-width: 1024px) {
    #overlay {
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        z-index: 0;
    }

    .main {
        width: 100%;
        display: flex;
        flex-direction: row;
        height: 100px;
        justify-content: space-between;
        align-items: center;
        padding: 0 3rem;
    }
    .input-wrap {
        display: flex;
        flex-direction: row;
        width: 551px;
        align-items: center;
    }
    .input {
        width: 250px;
        color: var(--input-text);
    }
    .hidden-wrap {
        width: 300px;
        font-size: 1.1rem;
        color: red;
    }
    .dropdown {
        justify-content: flex-end;
    }
    .dropdown-content a {
        text-align: left;
    }
}
</style>