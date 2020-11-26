<template>
    <div id="results">
        <p>{{this.results}}
            {{this.batterId}} and
            {{this.pitcherId}}
        </p>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "ResultsComponent",
    props: {
        batterId: String,
        pitcherId: String
    },
    data: function() {
        return {
            results: []
        }
    },
    watch: {
        batterId: function() {
            console.log("three")
            axios.get('https://api.blaseball-reference.com/v1/events', { params: { pitcherId: this.pitcherId, batterId: this.batterId} })
                 .then(response => {
                     if(response.status == 200) {
                         this.results = response['data']['results']
                     }
                     else {
                         console.log(response.error);
                     }
                 })
        }
    }
}
</script>

<style>

</style>