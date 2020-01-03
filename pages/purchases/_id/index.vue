<template>
  <b-container>
    <h4>Purchase Details:</h4>
    <p>Id: {{ purchase.id }}</p>
    <p>Coach Username: {{ purchase.partnerUsername }}</p>
    <p>Release Date: {{ purchase.releaseDate }}</p>
    <p>Price: {{ purchase.price }}</p>

    <h4>Products enrolled:</h4>
    <b-table v-if="products.length" striped over :items="products" :fields="fields" />
    <p v-else>Present athletes enrolled.</p>

    <h4>Payments Lists:</h4>
    <b-table striped over :items="payments" :fields="paymentsFields"/>

    <nuxt-link class="btn btn-info" to="/purchases">Back</nuxt-link>
    <nuxt-link class="btn btn-success" :to="`/purchases/${id}/addPayment`">Add Payment</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                purchase: {},
                fields: ['id', 'productTypeId', 'valueInEur'],
                products: [],
                payments: [],
                paymentsFields: ['id', 'stateId', 'receiptId','valueInEur']
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/${this.id}`)
                .then(purchase =>{
                    this.purchase = purchase || {};
                    this.products = purchase.products || [];
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/"+this.id+"/payments")
                .then(payments => {
                    this.payments = payments;
                });
        },
    }
</script>
