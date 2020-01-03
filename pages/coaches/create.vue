<template>
  <b-container>
  <div>
    <h1>Create a new Coach</h1>
    <form @submit.prevent="create">
        username: <b-input v-model="username" type="text"/>
        password: <b-input v-model="password" type="password"/>
        name: <b-input v-model="name" type="text"/>
        email: <b-input v-model="email" type="email"/>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>

      <button class="btn btn-danger" type="reset" @click="errorMsg = false">RESET</button>
      <button class="btn btn-success" @click.prevent="create">CREATE</button>
      <br><br>
      <nuxt-link class="btn btn-info" to="/coaches">Return</nuxt-link>
    </form>
  </div>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                username: null,
                password: null,
                name: null,
                email: null,
                errorMsg: false
            }
        },
        methods: {
            create() {
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/coaches', {
                    username: this.username,
                    password: this.password,
                    name: this.name,
                    email: this.email
                })
                    .then(() => {
                        this.$router.push('/coaches')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        }
    }
</script>
