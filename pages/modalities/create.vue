<template>
  <div>
    <h1>Create a new Administrator</h1>
    <form @submit.prevent="create">
      <div>
        name: <input v-model="name" type="text">
      </div>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/modalities">Return</nuxt-link>
      <button type="reset" @click="errorMsg = false">RESET</button>
      <button @click.prevent="create">CREATE</button>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                name: null
            }
        },
        methods: {
            create() {
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/modalities', {
                    name: this.name
                })
                    .then(() => {
                        this.$router.push('/modalities')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        }
    }
</script>
