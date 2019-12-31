<template>
  <b-container>
    <h4>Partner Details:</h4>
    <p>Username: {{ partner.username }}</p>
    <p>Name: {{ partner.name }}</p>
    <p>Email: {{ partner.email }}</p>
    <nuxt-link to="/partners">Back</nuxt-link>
    <nuxt-link :to="`/partners/${username}/payments`">Payments</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                partner: {},
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/partners/${this.username}`)
                .then(partner => this.partner = partner || {})
        },
    }
</script>
