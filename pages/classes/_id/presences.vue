<template>
  <b-container>
    <h4>Present athletes enrolled:</h4>
    <b-table v-if="athletesPresent.length" striped over :items="athletesPresent" :fields="athletesPresentFields">
      <template v-slot:cell(actions)="row">
        <button class="btn btn-link" v-on:click.prevent="athleteNotPresent(row.item.username)">Not Present</button>
      </template>
    </b-table>
    <p v-else>Present athletes enrolled.</p>

    <h4>Not present athletes enrolled:</h4>
    <b-table v-if="notPresent.length" striped over :items="notPresent" :fields="athletesPresentFields">
      <template v-slot:cell(actions)="row">
        <button class="btn btn-link" v-on:click.prevent="athletePresent(row.item.username)">Present</button>
      </template>
    </b-table>
    <p v-else>Not present athletes enrolled.</p>

    <nuxt-link class="btn btn-info" to="/classes">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                classe: {},
                notPresent:[],
                athletesPresent:[],
                athletesPresentFields: ['username', 'name',"email","actions"],
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        methods:{
            athleteNotPresent(username){

                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}/notPresent/`+username)
                    .then(classe =>{
                        this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}`)
                            .then(classe =>{
                                this.classe = classe || {};
                                this.athletesPresent = classe.athletesPresent;
                            });
                        this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}/notPresent`)
                            .then(notPresent => this.notPresent = notPresent || {});
                    });

            },
            athletePresent(username){
                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}/present/`+username)
                    .then(classe =>{
                        this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}`)
                            .then(classe =>{
                                this.classe = classe || {};
                                this.athletesPresent = classe.athletesPresent;
                            });
                        this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}/notPresent`)
                            .then(notPresent => this.notPresent = notPresent || {});
                    });
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}`)
                .then(classe =>{
                    this.classe = classe || {};
                    this.athletesPresent = classe.athletesPresent;
                });
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/classes/${this.id}/notPresent`)
                .then(notPresent => this.notPresent = notPresent || {});
        },
    }
</script>
