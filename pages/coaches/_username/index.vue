<template>
  <div>
    <b-container>
      <h4>Coach Details:</h4>
      <p>Username: {{ coach.username }}</p>
      <p>Name: {{ coach.name }}</p>
      <p>Email: {{ coach.email }}</p>
      <nuxt-link to="/athletes">Back</nuxt-link>

      <h4>Modalities enrolled:</h4>
      <b-table v-if="modalities.length"  striped over :items="modalities" :fields="modalitiesFields">
        <template v-slot:cell(details)="row">
          <nuxt-link class="btn btn-link" :to="`/coaches/${username}/${row.item.id}/schedules`">Schedule</nuxt-link>
        </template>
      </b-table>
      <p v-else>No modalities enrolled.</p>

      <h4>Echelons enrolled:</h4>
      <b-table v-if="echelons.length" striped over :items="echelons" :fields="echelonsFields" />
      <p v-else>No echelons enrolled.</p>

      <h4>Classes enrolled:</h4>
      <b-table v-if="classes.length" striped over :items="classes" :fields="classesFields" />
      <p v-else>No classes enrolled.</p>
    </b-container>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                coach: {},
                modalitiesFields: ['id','name','details'],
                echelonsFields: ["id","name","modalityId"],
                classesFields: ['id']
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            },
            modalities() {
                return this.coach.modalities || []
            },
            echelons() {
                return this.coach.echelons || []
            },
            classes(){
                return this.coach.classes || []
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/${this.username}`)
                .then(coach => this.coach = coach || {})
        },
    }
</script>
