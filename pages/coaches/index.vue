<template>
  <div>
    <b-container>
      <div>
        Username: <b-input v-model="username" type="text"/>
      </div>

      <div>
        Modality: <b-select v-model="modalityId" :options="modalities" required value-field="id" text-field="name">
        <template v-slot:first>
          <option :value="0" selected>None</option>
        </template>
      </b-select>
      </div>

      <div>
        <button class="btn btn-link" v-on:click.prevent="filterAthletes">Filter</button>
        <button class="btn btn-link" v-on:click.prevent="filterReset">Reset Filters</button>
      </div>
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
                coaches: [],
                username: "",
                modalityId: "0",
                modalities: []
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches")
                .then(coaches => {
                    this.coaches = coaches;
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities")
                .then(modalities => this.modalities = modalities);
        },
        methods: {
            destroy(username) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/${username}`).then(response => {
                    this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches")
                        .then(coaches => this.coaches = coaches);
                });
            },
            filterAthletes(){
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/filter", {
                    username: this.username,
                    modalityId: this.modalityId,
                }).then(coaches => this.coaches = coaches);
            },
            filterReset(){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches")
                    .then(coaches => {
                        this.coaches = coaches
                    });
            }
        }
    }
</script>
<style>
</style>
