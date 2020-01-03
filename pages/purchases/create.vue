

<template>
  <b-container>
    <div>


      <b-table striped over :items="products" :fields="fields">
        <template v-slot:cell(actions)="row">
          <button class="btn btn-link" v-on:click.prevent="addProduct(row.item.id)" >Add</button>
        </template>
      </b-table>


      <b-table striped over :items="addedProducts" :fields="fields">
        <template v-slot:cell(actions)="row">
          <button class="btn btn-link" v-on:click.prevent="removeProduct(row.item.id)" >Remove</button>
        </template>
      </b-table>


      <h1>Create a new Purchase</h1>
      <form @submit.prevent="create">
        Partner: <b-select v-model="partnerUsername" :options="partnersUsername" required value-field="username" text-field='username' v-on:change="getPartner(partnerUsername)">
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
                fields: ['id', 'productTypeId', 'valueInEur','stock','actions'],
                products:[],
                partner:{},
                partnerUsername:"",
                partnersUsername:[],
                releaseDate:"",
                price:0,
                addedProducts:[],
                errorMsg: false
            }
        },
        methods: {
            create() {
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/purchases', {
                    partnerUsername: this.partnerUsername,
                    releaseDate: this.releaseDate,
                    price: this.price,
                    products: this.addedProducts
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
            },
            addProduct(id){
                this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/products/${id}`)
                    .then(product =>{
                        this.addedProducts.push(product);
                    });
            },
            removeProduct(id){

                this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/products/${id}`)
                    .then(product =>{
                        this.addedProducts = this.addedProducts.filter(item => item.id !== product.id)
                    });
            }
        },
        created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/partners/")
                .then(partnersUsername => {
                    this.partnersUsername = partnersUsername;
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/products")
                .then(products => {
                    this.products = products;
                });
        }

    }
</script>
