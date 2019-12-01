<template>
  <div>
    <h1>Create a new Partner</h1>
    <form @submit.prevent="create">
      <div>
        username: <input v-model="username" type="text">
      </div>
      <div>
        password: <input v-model="password" type="password">
      </div>
      <div>
        name: <input v-model="name" type="text">
      </div>
      <div>
        email: <input v-model="email" type="email">
      </div>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/partners">Return</nuxt-link>
      <button type="reset" @click="errorMsg = false">RESET</button>
      <button @click.prevent="create">CREATE</button>
    </form>
  </div>
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
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/partners', {
                    username: this.username,
                    password: this.password,
                    name: this.name,
                    email: this.email
                })
                    .then(() => {
                        this.$router.push('/partners')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        }
    }
</script>
