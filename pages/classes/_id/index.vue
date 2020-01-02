<template>
  <b-container>
    <h4>Class Details:</h4>
    <p>Id: {{ classe.id }}</p>
    <p>Coach Username: {{ classe.coachUsername }}</p>

    <h4>Present athletes enrolled:</h4>
    <b-table v-if="athletesPresent.length" striped over :items="athletesPresent" :fields="athletesPresentFields" />
    <p v-else>Present athletes enrolled.</p>

    <nuxt-link to="/classes">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                classe: {},
                athletesPresent:[],
                athletesPresentFields: ['username', 'name',"email"],
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}`)
                .then(classe =>{
                    this.classe = classe || {}
                    this.athletesPresent = classe.athletesPresent
                });
        },
    }
</script>
