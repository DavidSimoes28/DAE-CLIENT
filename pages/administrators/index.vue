<template>
  <div>
    <b-container>
      <b-table striped over :items="administrators" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/administrators/${row.item.username}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/administrators/${row.item.username}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.username)" >Delete</button>
        </template>
      </b-table>
      <nuxt-link to="/">Back</nuxt-link>
    </b-container>
    <nuxt-link to="/administrators/create">Create a New Administrator</nuxt-link>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['username', 'name', 'email','actions'],
                administrators: []
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators")
                .then(administrators => {
                    this.administrators = administrators
                })
        },
        methods: {
            destroy(username) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/${username}`)
                window.location.reload()
            }
        }
    }
</script>
<style>
</style>
