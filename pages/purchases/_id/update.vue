<template>
  <b-container>
    <h4>Purchase Details:</h4>
    <p>Id: {{ purchase.id }}</p>
    <p>Coach Username: {{ purchase.partnerUsername }}</p>
    <p>Release Date: {{ purchase.releaseDate }}</p>

    Price: <b-input v-model="purchase.price" type="number"/>

    <h4>Products enrolled:</h4>
    <b-table v-if="products.length" striped over :items="products" :fields="fields" />
    <p v-else>Present athletes enrolled.</p>

    <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
    <button class="btn btn-danger" type="reset" @click="errorMsg = false">RESET</button>
    <button class="btn btn-success" @click.prevent="update">Update</button>
    <br><br>
    <nuxt-link class="btn btn-info" to="/purchases">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                purchase: {},
                fields: ['id', 'productTypeId', 'valueInEur'],
                products: [],
                errorMsg:""
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },methods:
            {
                update(){
                    this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/${this.id}`, this.purchase)
                        .then(() => {
                            this.$router.push('/purchases')
                        })
                        .catch(error => {
                            this.errorMsg = error.response.data
                        })
                }
            },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/${this.id}`)
                .then(purchase =>{
                    this.purchase = purchase || {};
                    this.products = purchase.products || [];
                });
        },
    }
</script>
