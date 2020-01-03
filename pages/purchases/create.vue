

<template>
  <b-container>
    <div>
      <h1>Create a new Purchase</h1>
      <form @submit.prevent="create">
        Product Type: <b-select v-model="partnerUsername" :options="partnersUsername" required value-field="username" text-field='username' v-on:change="getPartner(partnerUsername)">
        <template v-slot:first>
          <option :value="null" disabled>-- Please select the partner --</option>
        </template>
      </b-select>

        ReleaseDate: <b-input v-model="releaseDate" type="date"/>

        Price: <b-input v-model="price" type="number"/>


        <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
        <button class="btn btn-danger" type="reset" @click="errorMsg = false">RESET</button>
        <button class="btn btn-success" @click.prevent="create">CREATE</button>
        <br><br>
        <div><nuxt-link class="btn btn-info" to="/purchases">Return</nuxt-link></div>
      </form>
    </div>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                partner:{},
                partnerUsername:"",
                partnersUsername:[],
                releaseDate:"",
                price:0,
                errorMsg: false
            }
        },
        methods: {
            create() {
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/purchases', {
                    partnerUsername: this.partnerUsername,
                    releaseDate: this.releaseDate,
                    price: this.price
                })
                    .then(() => {
                        this.$router.push('/purchases')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            getPartner(partnerUsername){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/partners/"+partnerUsername)
                    .then(partner => {
                        this.partner = partner;
                    });
            }
        },
        created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/partners/")
                .then(partnersUsername => {
                    this.partnersUsername = partnersUsername;
                });
        }
    }
</script>
