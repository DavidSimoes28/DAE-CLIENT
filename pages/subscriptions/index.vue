<template>
  <div>
    <b-container>
      <b-table striped over :items="subscriptions" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/subscriptions/${row.item.id}`">Details</nuxt-link>
        </template>
      </b-table>
      <nuxt-link to="/">Back</nuxt-link>
    </b-container>
    <nuxt-link to="/subscriptions/create">Create a New Subscription</nuxt-link>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['id','athleteUsername',"echelonId","graduationId","subscriptionPrice",'actions'],
                subscriptions: []
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/subscriptions")
                .then(subscriptions => {
                    this.subscriptions = subscriptions;
                    if(this.subscriptions.echelonId == -1){
                        this.subscriptions.echelonId = ""
                    }
                    if(this.subscriptions.graduationId == -1){
                        this.subscriptions.graduationId = ""
                    }
                })
        },
        methods: {
        }
    }
</script>
<style>
</style>
