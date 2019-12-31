<template>
  <div>
    <b-container>
      <b-table striped over :items="coaches" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/coaches/${row.item.username}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/coaches/${row.item.username}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.username)" >Delete</button>
        </template>
      </b-table>
      <nuxt-link to="/">Back</nuxt-link>
      <nuxt-link to="/coaches/create">Create a New Coach</nuxt-link>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['username', 'name', 'email','actions'],
                coaches: []
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches")
                .then(coaches => {
                    this.coaches = coaches;
                })
        },
        methods: {
            destroy(username) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/${username}`)
                window.location.reload()
            }
        }
    }
</script>
<style>
</style>
