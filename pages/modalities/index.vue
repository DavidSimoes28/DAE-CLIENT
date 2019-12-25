<template>
  <div>
    <b-container>
      <b-table striped over :items="modalities" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/modalities/${row.item.id}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/modalities/${row.item.id}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.id)" >Delete</button>
        </template>
      </b-table>
      <nuxt-link to="/">Back</nuxt-link>
    </b-container>
    <nuxt-link to="/modalities/create">Create a New Modality</nuxt-link>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['id', 'name','actions'],
                modalities: []
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
                console.log(id)
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/${id}`)
                //window.location.reload()
            }
        }
    }
</script>
<style>
</style>
