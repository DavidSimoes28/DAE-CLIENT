<template>
  <b-container>
    <h4>Payments enrolled:</h4>
    <b-table v-if="payments.length" striped over :items="payments" :fields="paymentField">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" v-if="row.item.receiptId == 0" :to="`/partners/${username}/${row.item.id}`">Add Receipt</nuxt-link>
        <a v-else :href="`http://localhost:8080/SportsClubManagement_war_exploded/api/file/${row.item.receiptId}/download`" target="_blank">Download Receipt</a>
      </template>
    </b-table>
    <p v-else>Payments enrolled.</p>

    <nuxt-link :to="`/partners/${username}`">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                payments: {},
                paymentField: ['id', 'purchaseId',"stateId","actions"],
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/partners/${this.username}/payments`)
                .then(payments => this.payments = payments || {});
        },
    }
</script>
