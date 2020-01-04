<template>
  <b-container>
    <h4>Partner Details:</h4>
    <p>Username: {{ partner.username }}</p>
    <p>Name: {{ partner.name }}</p>
    <p>Email: {{ partner.email }}</p>
    <nuxt-link class="btn btn-info" to="/partners" v-show="this.auth">Back</nuxt-link>
    <nuxt-link class="btn btn-outline-primary" :to="`/partners/${username}/payments`">Payments</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                partner: {},
                auth:false
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
            this.auth = this.$auth.user.groups.includes('Administrator');
        },
    }
</script>
