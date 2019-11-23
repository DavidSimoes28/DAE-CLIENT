<template>
  <div>
    <h1>Update a Modality</h1>
    <form @submit.prevent="update">
      <div>
        name: <input v-model="modality.name" type="text">
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
                modality: {},
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        methods: {
            update() {
                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}`, {
                    id: this.modality.id,
                    name: this.modality.name
                })
                    .then(() => {
                        this.$router.push('/modalities')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}`)
                .then(modality => this.modality = modality || {})
        }
    }
</script>

