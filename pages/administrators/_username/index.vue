<template>
  <b-container>
    <h4>Student Details:</h4>
    <p>Username: {{ administrator.username }}</p>
    <p>Name: {{ administrator.name }}</p>
    <p>Email: {{ administrator.email }}</p>
    <nuxt-link class="btn btn-info" to="/administrators">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                administrator: {},
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/${this.username}`)
                .then(administrator => this.administrator = administrator || {})
        },
    }
</script>
