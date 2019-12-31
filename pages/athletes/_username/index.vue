<template>
  <div>
    <b-container>
      <h4>Athlete Details:</h4>
      <p>Username: {{ athletes.username }}</p>
      <p>Name: {{ athletes.name }}</p>
      <p>Email: {{ athletes.email }}</p>
      <nuxt-link to="/athletes">Back</nuxt-link>
      <nuxt-link :to="`/athletes/${username}/payments`">Payments</nuxt-link>

      <h4>Modalities enrolled:</h4>
      <b-table v-if="modalities.length"  striped over :items="modalities" :fields="modalitiesFields">
        <template v-slot:cell(details)="row">
          <nuxt-link class="btn btn-link" :to="`/athletes/${username}/${row.item.id}/schedules`">Schedule</nuxt-link>
        </template>
      </b-table>
      <p v-else>No modalities enrolled.</p>

      <h4>Echelons enrolled:</h4>
      <b-table v-if="echelons.length" striped over :items="echelons" :fields="echelonsFields" />
      <p v-else>No echelons enrolled.</p>

      <h4>Graduations enrolled:</h4>
      <b-table v-if="graduations.length" striped over :items="graduations" :fields="graduations" />
      <p v-else>No graduations enrolled.</p>
    </b-container>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                athletes: {},
                modalitiesFields: ['id','name','details'],
                echelonsFields: ["id","name","modalityId"],
                graduationsFields: ['id']
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            },
            modalities() {
                return this.athletes.modalities || []
            },
            echelons() {
                return this.athletes.echelons || []
            },
            graduations() {
                return this.athletes.graduations || []
            },
            schedules(){
                return this.athletes.schedules || []
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/athletes/${this.username}`)
                .then(athletes => this.athletes = athletes || {})
        },
    }
</script>
