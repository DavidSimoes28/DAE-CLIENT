<template>
  <div>
    <b-container>
      <b-table striped over :items="classes" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/classes/${row.item.id}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/classes/${row.item.id}/presences`">Presences</nuxt-link>
          <!--<button class="btn btn-link" v-on:click.prevent="destroy(row.item.id)" >Delete</button>-->
        </template>
      </b-table>
      <nuxt-link class="btn btn-secondary" to="/classes/create">Create a New Modality</nuxt-link>
      <div><nuxt-link class="btn btn-info" to="/">Back</nuxt-link></div>
    </b-container>

  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['id','modalityID',"coachUsername","date","actions"],
                classes: []
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/classes")
                .then(classes => {
                    this.classes = classes
                })
        },
        methods: {
            /*destroy(id) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${id}`)
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/classes")
                    .then(classes => {
                        this.classes = classes
                    })
            }*/
        }
    }
</script>
<style>
</style>
