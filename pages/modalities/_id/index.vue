<template>
  <b-container>
    <h4>Modality Details:</h4>
    <p>Id: {{ modality.id }}</p>
    <p>Name: {{ modality.name }}</p>
    <nuxt-link to="/modalities">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                modality: {},
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}`)
                .then(modality => this.modality = modality || {})
        },
    }
</script>
