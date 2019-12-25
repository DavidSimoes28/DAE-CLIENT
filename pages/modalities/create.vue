<template>
  <div>
    <h1>Create a new Administrator</h1>
    <form @submit.prevent="create">
      <div>
        Name: <input v-model="name" type="text">
      </div>
      <div>
        SportsYear: <input v-model="sportYear" type="number">
      </div>
      <div>
        Active: <b-select v-model="active">
        <template v-slot:first>
          <option :value="null" disabled>-- Please select the Course --</option>
        </template>
        <option :value="true">Active</option>
        <option :value="false">Deactivated</option>
      </b-select>
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
                name: null,
                sportYear: null,
                active: null,
                errorMsg:""
            }
        },
        methods: {
            create() {
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/', {
                    name: this.name,
                    sportYear: this.sportYear,
                    active: this.active
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
