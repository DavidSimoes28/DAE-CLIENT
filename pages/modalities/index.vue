<template>
  <div>
    <b-container>
      <div>
        Name: <b-input v-model="name" type="text"/>
      </div>
      <div>
        <button class="btn btn-outline-primary" v-on:click.prevent="filterModality">Filter</button>
        <button class="btn btn-outline-primary" v-on:click.prevent="filterReset">Reset Filters</button>
      </div>
      <b-table striped over :items="modalities" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/modalities/${row.item.id}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/modalities/${row.item.id}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.id)" >Delete</button>
        </template>
      </b-table>
      <nuxt-link class="btn btn-secondary" to="/modalities/create">Create a New Modality</nuxt-link>
      <div><nuxt-link class="btn btn-info" to="/">Back</nuxt-link></div>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['id', 'name','actions'],
                modalities: [],
                name:""
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities")
                .then(modalities => {
                    this.modalities = modalities
                })
        },
        methods: {
            destroy(id) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${id}`);
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities")
                    .then(modalities => {
                        this.modalities = modalities
                    });
            },
            filterModality(){
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/filter", {
                    name: this.name,
                }).then(modalities => this.modalities = modalities);
            },
            filterReset(){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities")
                    .then(modalities => {
                        this.modalities = modalities
                    });
            }
        }
    }
</script>
<style>
</style>
