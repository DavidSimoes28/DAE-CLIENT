<template>
  <div>
    <b-container>
    <h1>Update a Administrator</h1>
      <form @submit.prevent="update">
        <div>
          name: <b-input v-model="administrator.name" type="text"/>
        </div>
        <div>
          password: <b-input v-model="administrator.password" type="password"/>
        </div>
        <div>
          email: <b-input v-model="administrator.email" type="email"/>
        </div>
        <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
        <button class="btn btn-danger" type="reset" @click="errorMsg = false">RESET</button>
        &nbsp;
        <button class="btn btn-success"  @click.prevent="update">Update</button>
        <br><br>
        <div><nuxt-link class="btn btn-info" to="/administrators">Return</nuxt-link></div>
      </form>
    </b-container>
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

