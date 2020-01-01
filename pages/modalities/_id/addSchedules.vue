<template>
  <b-container>
    <h4>Schedules:</h4>
    <b-table v-if="schedules.length" striped over :items="schedules" :fields="schedulesFields">
      <template v-slot:cell(startDate)="row">
        <b v-if="row.item.startDate.minutes < 10">{{row.item.startDate.hour+":"+row.item.startDate.minutes+"0"}}</b>
        <b v-else>{{row.item.startDate.hour+":"+row.item.startDate.minutes}}</b>
      </template>
      <template v-slot:cell(endDate)="row">
        <b v-if="row.item.endDate.minutes < 10">{{row.item.endDate.hour+":"+row.item.endDate.minutes+"0"}}</b>
        <b v-else>{{row.item.endDate.hour+":"+row.item.endDate.minutes}}</b>
      </template>
      <template v-slot:cell(addSchedule)="row">
        <button class="btn btn-link" v-on:click.prevent="addSchedule(row.item.id)" >Add Schedule</button>
      </template>
    </b-table>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                modality: {},
                schedules: [],
                coachFields: ['username', 'name', 'email'],
                athleteFields: ['username', 'name', 'email'],
                schedulesFields: ['id','startDate','endDate','addSchedule']
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        methods:{
          addSchedule(id){
            this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}/add/schedule/${id}`)
                .then(() => {
                    this.$router.push(`/modalities/${this.id}`)
                });
          }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}`)
                .then(modality => this.modality = modality || {});
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}/unassigned/schedules`)
                .then(schedules => this.schedules = schedules || {})
        },
    }
</script>
