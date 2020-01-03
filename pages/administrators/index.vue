<template>
  <div>
    <b-container>
      <div>
        Username: <b-input v-model="username" type="text"/>
      </div>
      <div>
        <button class="btn btn-link" v-on:click.prevent="filterPartners">Filter</button>
        <button class="btn btn-link" v-on:click.prevent="filterReset">Reset Filters</button>
      </div>
      <b-table striped over :items="administrators" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/administrators/${row.item.username}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/administrators/${row.item.username}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.username)" >Delete</button>
        </template>
      </b-table>

      <nuxt-link class="btn btn-secondary" to="/administrators/create">Create a New Administrator</nuxt-link>
      &nbsp;
      <nuxt-link class="btn btn-secondary" to="/administrators/email-send">Send-email</nuxt-link>
      <div><nuxt-link class="btn btn-info" to="/">Back</nuxt-link></div>
    </b-container>

  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['username', 'name', 'email','actions'],
                administrators: [],
                username: ""
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
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/${username}`);
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators")
                    .then(administrators => {
                        this.administrators = administrators
                    });
            },
            filterPartners(){
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/filter", {
                    username: this.username,
                }).then(administrators => this.administrators = administrators);
            },
            filterReset(){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators")
                    .then(administrators => {
                        this.administrators = administrators
                    });
            }
        }
    }
</script>
<style>
</style>
