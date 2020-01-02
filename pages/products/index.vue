<template>
  <div>
    <b-container>
      <!--<div>
        Username: <b-input v-model="username" type="text"/>
      </div>
      <div>
        <button class="btn btn-link" v-on:click.prevent="filterPartners">Filter</button>
        <button class="btn btn-link" v-on:click.prevent="filterReset">Reset Filters</button>
      </div>-->

      <b-table striped over :items="products" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/products/${row.item.id}`">Details</nuxt-link>
          <!--<nuxt-link class="btn btn-link" :to="`/partners/${row.item.username}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.username)" >Delete</button>-->
        </template>
      </b-table>
      <div><nuxt-link to="/">Back</nuxt-link></div>
      <div><nuxt-link to="/products/create">Create a New Product</nuxt-link></div>
    </b-container>

  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['id', 'productTypeId', 'valueInEur','stock','actions'],
                products: [],
                username: "",
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/products")
                .then(products => {
                    this.products = products;
                })
        },
        methods: {
            /*destroy(username) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/partners/${username}`);
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/partners")
                    .then(partners => this.partners = partners);
            },
            filterProduct(){
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/partners/filter", {
                    username: this.username,
                }).then(partners => this.partners = partners);
            },
            filterReset(){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/partners")
                    .then(partners => {
                        this.partners = partners
                    });
            }*/
        }
    }
</script>
<style>
</style>
