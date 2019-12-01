<template>
  <div>
    <h1>Update a Partner</h1>
    <form @submit.prevent="update">
      <div>
        name: <input v-model="partner.name" type="text">
      </div>
      <div>
        password: <input v-model="partner.password" type="password">
      </div>
      <div>
        email: <input v-model="partner.email" type="email">
      </div>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/partners">Return</nuxt-link>
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
                partner: {},
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        methods: {
            update() {
                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/partners/${this.username}`, {
                    username: this.partner.username,
                    password: this.partner.password,
                    name: this.partner.name,
                    email: this.partner.email
                })
                    .then(() => {
                        this.$router.push('/partners')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/partners/${this.username}`)
                .then(partner => this.partner = partner || {})
        }
    }
</script>

