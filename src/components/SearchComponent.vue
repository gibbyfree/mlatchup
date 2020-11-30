<template>
    <div id ="search">
        <input v-model="batterName" placeholder="batter name">
        <input v-model="pitcherName" placeholder="pitcher name">
        <button id="search-button" @click="executeSearch();">
            Search
        </button>
        {{this.results}}
    </div>
</template>

<script>
import axios from 'axios';


export default {
    data: function() {
        return {
            batterName: "",
            pitcherName: "",
            pitcherId: "",
            batterId: "", 
            results: []
        }
    },
    components: {},
    methods: {
        executeSearch: function() {
            this.getPitcherIdByName();            
        },
        getResults: function() {
            axios.get('https://api.blaseball-reference.com/v1/events', 
                { params: { pitcherId: this.pitcherId, 
                            batterId: this.batterId} })
                 .then(response => {
                     if(response.status == 200) {
                         this.results = response['data']['results']
                         console.log("got results")
                     }
                     else {
                         console.log(response.error);
                     }
                 })
        },
        getPitcherIdByName: function() {
            axios.get('https://api.blaseball-reference.com/v1/playerIdsByName', { params: { name: this.pitcherName, current: true} })
                 .then(response => {
                     if(response.status == 200) {
                         this.pitcherId = response['data'][0]['player_id']
                         console.log("one")
                         this.getBatterIdByName();
                     }
                     else {
                         console.log(response.error);
                     }
                 })
        },
        getBatterIdByName: function() {
            axios.get('https://api.blaseball-reference.com/v1/playerIdsByName', { params: { name: this.batterName, current: true} })
                 .then(response => {
                     if(response.status == 200) {
                        this.batterId = response['data'][0]['player_id']
                        console.log("two")
                        this.getResults();
                     }
                     else {
                         console.log(response.error)
                     }
                 })
        }
    },
    watch: {
        results: function() {
             this.$forceUpdate();
        }

    }
}
</script>

<style>

</style>