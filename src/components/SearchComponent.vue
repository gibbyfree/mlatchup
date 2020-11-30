<template>
    <div id ="search">
        <input v-model="batterName" placeholder="batter name">
        <input v-model="pitcherName" placeholder="pitcher name">
        <button id="search-button" @click="executeSearch();">
            Search
        </button>
        <div v-if="ready">
            <ul v-for="result in this.results" :key="result.id">
                <li v-for="event in result" :key="event.id">
                    {{event.event_type}}
                </li>
            </ul>
            {{this.results}}
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import _ from 'lodash';


export default {
    data: function() {
        return {
            batterName: "",
            pitcherName: "",
            pitcherId: "",
            batterId: "", 
            results: [],
            ready: false
        }
    },
    components: {},
    methods: {
        executeSearch: function() {
            // First, get the pitcher's ID. 
            this.getPitcherIdByName();            
        },
        getResults: function() {
            axios.get('https://api.blaseball-reference.com/v1/events', 
                { params: { pitcherId: this.pitcherId, 
                            batterId: this.batterId} })
                 .then(response => {
                     if(response.status == 200) {
                         this.results = response['data']['results']
                         // Group the data by games.
                         this.results = _.groupBy(this.results, 'game_id')
                         // Results are ready to render.
                         this.ready = true
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
                         // Second, get the batter's ID. 
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
                        // Get the event data from the API with these IDs. 
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