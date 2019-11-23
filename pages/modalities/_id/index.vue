<template>
  <b-container>
    <h4>Modality Details:</h4>
    <p>Id: {{ modality.id }}</p>
    <p>Name: {{ modality.name }}</p>
    <h4>Coaches enrolled:</h4>
    <b-table v-if="coaches.length" striped over :items="coaches" :fields="fields" />
    <p v-else>No coaches enrolled.</p>
    <nuxt-link to="/modalities">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                modality: {},
                fields: ['username', 'name', 'email'],
                coaches: []
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            },
            coaches() {
                return this.modality.coaches || []
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}`)
                .then(modality => this.modality = modality || {})
        },
    }
</script>
