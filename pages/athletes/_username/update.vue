<template>
  <div>
    <h1>Update a Athlete</h1>
    <form @submit.prevent="update">
      <div>
        name: <input v-model="athlete.name" type="text">
      </div>
      <div>
        password: <input v-model="athlete.password" type="password">
      </div>
      <div>
        email: <input v-model="athlete.email" type="email">
      </div>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/athletes">Return</nuxt-link>
      <button type="reset" @click="errorMsg = false">RESET</button>
      <button @click.prevent="update">Update</button>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                errorMsg: false,
                athlete: {},
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        methods: {
            update() {
                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/athletes/${this.username}`, {
                    username: this.athlete.username,
                    password: this.athlete.password,
                    name: this.athlete.name,
                    email: this.athlete.email
                })
                    .then(() => {
                        this.$router.push('/athletes')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/athletes/${this.username}`)
                .then(athlete => this.athlete = athlete || {})
        }
    }
</script>

