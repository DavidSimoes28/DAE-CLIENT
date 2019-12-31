<template>
  <b-container>
    <h4>Modality Details:</h4>
    <p>Id: {{ modality.id }}</p>
    <p>Name: {{ modality.name }}</p>

    <h4>Coaches enrolled:</h4>
    <b-table v-if="coaches.length" striped over :items="coaches" :fields="coachFields" />
    <p v-else>No coaches enrolled.</p>

    <h4>Athletes enrolled:</h4>
    <b-table v-if="athletes.length" striped over :items="athletes" :fields="athleteFields" />
    <p v-else>No athletes enrolled.</p>

    <h4>Schedules enrolled:</h4>
    <b-table v-if="schedules.length" striped over :items="schedules" :fields="schedulesFields">
      <template v-slot:cell(startDate)="row">
        <b v-if="row.item.startDate.minutes < 10">{{row.item.startDate.hour+":"+row.item.startDate.minutes+"0"}}</b>
        <b v-else>{{row.item.startDate.hour+":"+row.item.startDate.minutes}}</b>
      </template>
      <template v-slot:cell(endDate)="row">
        <b v-if="row.item.endDate.minutes < 10">{{row.item.endDate.hour+":"+row.item.endDate.minutes+"0"}}</b>
        <b v-else>{{row.item.endDate.hour+":"+row.item.endDate.minutes}}</b>
      </template>
    </b-table>
    <p v-else>No schedules enrolled.</p>

    <nuxt-link class="btn btn-link" :to="`/modalities/${id}/addSchedules`">Add Schedules</nuxt-link>
    <nuxt-link to="/modalities">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                modality: {},
                coachFields: ['username', 'name', 'email'],
                athleteFields: ['username', 'name', 'email'],
                schedulesFields: ['id','startDate','endDate']
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            },
            coaches() {
                return this.modality.coaches || []
            },
            athletes() {
                return this.modality.athletes || []
            },
            schedules() {
                return this.modality.schedules || []
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}`)
                .then(modality => this.modality = modality || {})
        },
    }
</script>
