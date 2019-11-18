<template>
  <form @submit.prevent="destroy">
    <button @click.prevent="destroy">Delete</button>
  </form>
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
        methods: {
            destroy() {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/${this.username}`)
                    .then(() => {
                        this.$router.push('/administrators')
                    })
            }
        }
    }
</script>
