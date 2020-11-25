<template>
    <div id ="search">
        <input v-model="batterName" placeholder="batter name">
        <input v-model="pitcherName" placeholder="pitcher name">
        <button id="search-button" @click="getBatterIdByName(); getPitcherIdByName();">
            Search
        </button>
        <p>
            {{this.pitcherId}} and {{this.batterId}}
        </p>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data: function() {
        return {
            results: [],
            batterName: "",
            pitcherName: "",
            batterId: "",
            pitcherId: ""
        }
    },
    methods: {
        getPitcherIdByName: function() {
            axios.get('https://api.blaseball-reference.com/v1/playerIdsByName', { params: { name: this.pitcherName, current: true} })
                 .then(response => {
                     if(response.status == 200) {
                         this.pitcherId = response['data'][0]['player_id']
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