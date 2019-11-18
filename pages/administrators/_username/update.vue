<template>
  <div>
    <h1>Update a Administrator</h1>
    <form @submit.prevent="update">
      <div>
        name: <input v-model="administrator.name" type="text">
      </div>
      <div>
        password: <input v-model="administrator.password" type="password">
      </div>
      <div>
        email: <input v-model="administrator.email" type="email">
      </div>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/administrators">Return</nuxt-link>
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
                administrator: {},
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        methods: {
            update() {
                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/${this.username}`, {
                    username: this.administrator.username,
                    password: this.administrator.password,
                    name: this.administrator.name,
                    email: this.administrator.email
                })
                    .then(() => {
                        this.$router.push('/administrators')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/${this.username}`)
                .then(administrator => this.administrator = administrator || {})
        }
    }
</script>

