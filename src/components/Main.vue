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
           <Countries></Countries>
        </div>
    </div>
</template>

<script>
import Countries from '@/components/Countries.vue'

export default {
    name: 'Main',
    components: {
        Countries
    },
    data() {
      return {
        query: "",
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
                
                const myStorage = window.localStorage;
                myStorage.setItem('country', this.query);
                this.$router.push({ path: 'country' });

                // console.log(this.json)
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