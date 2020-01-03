

<template>
  <b-container>
    <div>
      <h1>Create a new Payment</h1>
      <form @submit.prevent="create">
        State: <b-select v-model="stateId" :options="states" required value-field="id" text-field='name' v-on:change="getState(stateId)">
        <template v-slot:first>
          <option :value="null" disabled>-- Please select the partner --</option>
        </template>
      </b-select>
        Price:
        <b-input type="number" v-model="valueInEur"/>
        <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
        <button class="btn btn-danger" type="reset" @click="errorMsg = false">RESET</button>
        <button class="btn btn-success" @click.prevent="create">CREATE</button>
        <br><br>
        <div><nuxt-link class="btn btn-info" to="/purchases">Return</nuxt-link></div>
      </form>

      <b-table striped over :items="payments" :fields="fields"/>
    </div>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                fields: ['id', 'stateId', 'receiptId','valueInEur'],
                purchase:{},
                payments:[],
                state: {},
                states: [],
                payment:{},
                stateId:"0",
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
            create() {
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/payments', {
                    purchaseId: this.id,
                    stateId: this.stateId,
                    receiptId: this.receiptId,
                    valueInEur: this.valueInEur
                })
                    .then(() => {
                        this.$router.push(`/purchases/${this.id}`)
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            getState(id){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/states/"+id)
                    .then(state => {
                        this.state = state;
                    });
            }
        },
        created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/"+this.id+"/payments")
                .then(payments => {
                    this.payments = payments;
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/states")
                .then(states => {
                    this.states = states;
                });
        }

    }
</script>
