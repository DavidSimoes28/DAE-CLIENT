<template>
  <div>
    <h1>Create a new Coach</h1>
    <form @submit.prevent="create">
      <div>
        username: <input v-model="coach.username" type="text">
      </div>
      <div>
        name: <input v-model="coach.name" type="text">
      </div>
      <div>
        email: <input v-model="coach.email" type="email">
      </div>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/coaches">Return</nuxt-link>
      <button type="reset" @click="errorMsg = false">RESET</button>
      <button @click.prevent="update">UPDATE</button>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                coach:{},
                errorMsg: false
            }
        },
        methods: {
            update() {
                this.$axios.$put('http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/'+this.coach.username, {
                    username: this.coach.username,
                    name: this.coach.name,
                    email: this.coach.email
                })
                    .then(() => {
                        this.$router.push('/coaches')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        },
    computed: {
        username() {
            return this.$route.params.username
        }
    },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/${this.username}`)
                .then(coach => this.coach = coach || {})
        }
    }
</script>
