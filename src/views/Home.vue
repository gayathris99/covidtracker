<template>
    <main v-if="!loading">
        <DataTitle :text="title" :dataDate="dataDate"/>
        <DataBoxes :stats="stats"/>
        <CountrySelect :countries="countries" @get-country="getCountryData"/>
        <button  v-if="stats.Country" @click="clearCountry" class="bg-green-700 text-white rounded hover:bg-green-600 p-3 mt-10 focus:outline-none">Clear Country</button>
    </main>
    <main class="column align-center justify-center text-center" v-else>
        <div class="text-gray-500 text-3xl mt-10 mb-6">
            Fetching Data
        </div>
        <img :src="loadingImage" class="w-24 m-auto" alt="">
    </main>
</template>

<script>
import DataTitle from '../components/DataTitle.vue'
import DataBoxes from '../components/DataBoxes.vue'
import CountrySelect from '../components/CountrySelect.vue'
export default {
    name: "Home",
    components: {DataTitle,DataBoxes,CountrySelect},
    data() {
        return {
            loading: true,
            loadingImage: require('../assets/hourglass.gif'),
            title: 'Global',
            dataDate: "",
            stats: {},
            countries: []
        }
    },
    methods: {
        async fetchCovidData() {
            const result = await fetch("https://api.covid19api.com/summary")
            const data = await result.json()
            return data
        },
        async clearCountry() {
            this.loading = true
            const data = await this.fetchCovidData()
            this.dataDate = data.Date 
            this.title = "Global"
            this.stats = data.Global
            this.countries = data.Countries
            this.loading = false
        },
        getCountryData(country) {
            this.title = country.Country
            this.stats = country
        }
    },
    async mounted() {
        const data = await this.fetchCovidData()
        this.dataDate = data.Date 
        this.stats = data.Global
        this.countries = data.Countries
        this.loading = false
    }

}
</script>