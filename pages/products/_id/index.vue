<template>
  <b-container>
    <h4>Product Details:</h4>
    <p>Id: {{ product.id }}</p>
    <p>Product Type: {{ productType.type }}</p>
    <p>Description: {{ product.description }}</p>
    <p>Value: {{ product.valueInEur }}</p>
    <p>Stock: {{ product.stock }}</p>

    <!--<h4>Present athletes enrolled:</h4>
    <b-table v-if="athletesPresent.length" striped over :items="athletesPresent" :fields="athletesPresentFields" />
    <p v-else>Present athletes enrolled.</p>-->

    <nuxt-link class="btn btn-info" to="/products">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                product: {},
                productType:{},
                athletesPresent:[],
                athletesPresentFields: ['username', 'name',"email"],
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/products/${this.id}`)
                .then(product =>{
                    this.product = product || {};
                    this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/productTypes/${this.product.productTypeId}`)
                        .then(productType =>{
                            this.productType = productType || {};
                        });
                });
        },
    }
</script>
