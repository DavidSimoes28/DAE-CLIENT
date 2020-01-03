<template>
  <div>
    <b-container>
      <div>
        Product:<b-input v-model="productId" type="text"/>

        ProductTypeId: <b-select v-model="productTypeId" :options="productTypes" required value-field="id" text-field='type'>
        <template v-slot:first>
          <option :value="0">None</option>
        </template>
      </b-select>
      </div>
      <div>
        <button class="btn btn-outline-primary" v-on:click.prevent="filterProduct">Filter</button>
        <button class="btn btn-outline-primary" v-on:click.prevent="filterReset">Reset Filters</button>
      </div>

      <b-table striped over :items="products" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/products/${row.item.id}`">Details</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/products/${row.item.id}/update`">Update</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.id)" >Delete</button>
        </template>
      </b-table>
      <div><nuxt-link class="btn btn-secondary" to="/products/create">Create a New Product</nuxt-link></div>
      <div><nuxt-link class="btn btn-info" to="/">Back</nuxt-link></div>
    </b-container>

  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['id', 'productTypeId', 'valueInEur','stock','actions'],
                products: [],
                productTypeId:"0",
                productId:"0",
                productTypes:[]
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/products")
                .then(products => {
                    this.products = products;
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/productTypes/")
                .then(productTypes => {
                    this.productTypes = productTypes;
                });
        },
        methods: {
            destroy(id) {
                this.$axios.$delete(`http://localhost:8080/SportsClubManagement_war_exploded/api/products/${id}`)
                    .then(() => {
                        this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/products")
                            .then(products => {
                                this.products = products;
                            })
                    }).catch(()=>{
                        this.$toast.error("Can't delete this product")
                })

            },
            filterProduct(){
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/products/filter", {
                    id: this.productId,
                    productTypeId: this.productTypeId
                }).then(products => this.products = products);
            },
            filterReset(){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/products")
                    .then(products => {
                        this.products = products
                    });
            }
        }
    }
</script>
<style>
</style>
