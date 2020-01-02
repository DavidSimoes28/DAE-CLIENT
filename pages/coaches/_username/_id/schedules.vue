<template>
  <b-container>
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

    <nuxt-link :to="`/coaches/${this.username}`">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                schedules: [],
                schedulesFields: ['id','startDate','endDate']
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            },
            username() {
                return this.$route.params.username
            },
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${this.id}/coach/${this.username}/schedules`)
                .then(schedules => this.schedules = schedules || [])
        },
    }
</script>
