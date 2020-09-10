<template>
    <div>
        <div class="main">
            <div>
                <input 
                    type="text" 
                    class="input is-rounded"
                    placeholder="Search for a country..." 
                    v-model="query"
                    @keyup.enter="getCountry()"
                />
            </div>
            <div class="dropdown">
            <!-- <div class="dropdown is-active"> -->
                <div class="dropdown-trigger">
                        <button class="button" aria-haspopup="true" aria-controls="dropdown-menu">
                        <span>Filter by Region</span>
                        <span class="icon is-small">
                            <i class="fas fa-angle-down" aria-hidden="true"></i>
                        </span>
                        </button>
                    </div>
                    <div class="dropdown-menu" id="dropdown-menu" role="menu">
                        <div class="dropdown-content">
                            <a href="#" class="dropdown-item">
                                Africa
                            </a>
                            <a class="dropdown-item">
                                America
                            </a>
                            <a href="#" class="dropdown-item is-active">
                                Asia
                            </a>
                            <a href="#" class="dropdown-item">
                                Europe
                            </a>
                            <a href="#" class="dropdown-item">
                                Oceania
                            </a>
                            <hr class="dropdown-divider">
                            <a href="#" class="dropdown-item">
                                All
                            </a>
                        </div>
                    </div>
                </div>
        </div>
        <div>
            <Country :json="json" v-if="json"></Country>
        </div>
    </div>
</template>

<script>
import Country from '@/components/Country.vue'

export default {
    name: 'Main',
    components: {
        Country
    },
    data() {
      return {
        query: "",
        json: "",
        error: ""
      }
    },
    methods: {
      getCountry() {
        fetch(`https://restcountries.eu/rest/v2/name/${this.query}`)
            .then(res => {
                return res.json();
            }).then(result => {
                this.json = result;
                console.log(this.json)
            }).catch((err) => {
                this.error = err;
            })
      }
    }
}
</script>

<style>
.main {
    widows: 100%;
    display: flex;
    height: 100px;
    justify-content: space-between;
    align-items: center;
    padding: 0 5rem;
}
</style>