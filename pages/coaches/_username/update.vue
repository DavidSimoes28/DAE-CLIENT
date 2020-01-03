<template>
  <b-container>
  <div>
    <h1>Create a new Coach</h1>
    <form @submit.prevent="create">
        username: <b-input v-model="coach.username" type="text"/>
        name: <b-input v-model="coach.name" type="text"/>
        email: <b-input v-model="coach.email" type="email"/>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <button class="btn btn-danger" type="reset" @click="errorMsg = false">RESET</button>
      <button class="btn btn-success" @click.prevent="update">UPDATE</button>
      <br><br>
      <div><nuxt-link class="btn btn-info" to="/coaches">Return</nuxt-link></div>

    </form>
  </div>
  </b-container>
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
