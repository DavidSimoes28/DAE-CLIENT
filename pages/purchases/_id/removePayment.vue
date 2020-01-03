

<template>
  <b-container>
      <h1>Remove a Payment</h1>

          <b-table striped over :items="payments" :fields="fields">
            <template v-slot:cell(remove)="row">
              <button class="btn btn-link" v-on:click.prevent="destroy(row.item.id)" >Delete</button>
            </template>
          </b-table>

        <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
        <br><br>
        <div><nuxt-link class="btn btn-info" :to="`/purchases/${this.id}`">Return</nuxt-link></div>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                fields: ['id', 'stateId', 'receiptId','valueInEur',"remove"],
                purchase:{},
                payments:[],
                state: {},
                states: [],
                payment:{},
                paymentId:"0",
                receiptId:"0",
                valueInEur:"0",
                errorMsg: false
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        methods: {
            destroy(paymentId) {
                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/${this.id}/remove/payment/${paymentId}`)
                    .then(() => {
                        this.$router.push(`/purchases/${this.id}`)
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        },
        created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/"+this.id+"/payments")
                .then(payments => {
                    this.payments = payments;
                });
        }

    }
</script>
