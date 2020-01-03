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
        Echelon: <b-select v-model="echelonId" :options="echelons" required value-field="id" text-field="name">
          <template v-slot:first>
            <option :value="0" selected>None</option>
          </template>
        </b-select>
      </div>

      <div>
        Graduation: <b-select v-model="graduationId" :options="graduations" required value-field="id" text-field="name">
          <template v-slot:first>
            <option :value="0" selected>None</option>
          </template>
        </b-select>
      </div>

      <div>
        <button class="btn btn-outline-primary" v-on:click.prevent="filterAthletes">Filter</button>
        <button class="btn btn-outline-primary" v-on:click.prevent="filterReset">Reset Filters</button>
      </div>
      <b-table striped over :items="athletes" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/athletes/${row.item.username}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/athletes/${row.item.username}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.username)" >Delete</button>
        </template>
      </b-table>
      <nuxt-link class="btn btn-secondary" to="/athletes/create">Create a New Athlete</nuxt-link>
      <div><nuxt-link class="btn btn-info" to="/">Back</nuxt-link></div>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['username', 'name', 'email','actions'],
                athletes: [],
                username: "",
                modalityId: 0,
                echelonId: 0,
                graduationId: 0,
                modalities: [],
                echelons: [],
                graduations: []
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/athletes")
                .then(athletes => this.athletes = athletes);
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities")
                .then(modalities => this.modalities = modalities);
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/echelons")
                .then(echelons => this.echelons = echelons);
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/graduations")
                .then(graduations => this.graduations = graduations);
        },
        methods: {
            destroy(username) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/athletes/${username}`).then(response => {
                    this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/athletes")
                        .then(athletes => this.athletes = athletes);
                });
            },
            filterAthletes(){
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/athletes/filter", {
                    username: this.username,
                    modalityId: this.modalityId,
                    echelonId: this.echelonId,
                    graduationId: this.graduationId
                }).then(athletes => this.athletes = athletes);
            },
            filterReset(){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/athletes")
                    .then(athletes => {
                        this.athletes = athletes
                    });
            }
        }
    }
</script>
<style>
</style>
