<template>
    <div id ="search">
        <input v-model="batterName" placeholder="batter name">
        <input v-model="pitcherName" placeholder="pitcher name">
        <button id="search-button" @click="getBatterIdByName(); getPitcherIdByName();">
            Search
        </button>
        <ResultsComponent v-bind:pitcherId='this.pitcherId' v-bind:batterId='this.batterId'/>
    </div>
</template>

<script>
import axios from 'axios';
import ResultsComponent from "./ResultsComponent";


export default {
    data: function() {
        return {
            batterName: "",
            pitcherName: "",
            pitcherId: "",
            batterId: ""
        }
    },
    components: {
        ResultsComponent
    },
    methods: {
        getPitcherIdByName: function() {
            axios.get('https://api.blaseball-reference.com/v1/playerIdsByName', { params: { name: this.pitcherName, current: true} })
                 .then(response => {
                     if(response.status == 200) {
                         this.pitcherId = response['data'][0]['player_id']
                         console.log("one")
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
                     }
                     else {
                         console.log(response.error)
                     }
                 })
        }
    }
}
</script>

<style>

</style>