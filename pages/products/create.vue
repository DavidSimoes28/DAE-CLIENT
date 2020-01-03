

<template>
  <b-container>
  <div>
    <h1>Create a new Product</h1>
    <form @submit.prevent="create">
       Product Type: <b-select v-model="productTypeId" :options="productTypes" required value-field="id" text-field='type' v-on:change="getProductType(productTypeId)">
      <template v-slot:first>
        <option :value="null" disabled>-- Please select the athlete --</option>
      </template>
    </b-select>

        Description: <b-input v-model="description" type="text"/>

        Value: <b-input v-model="valueInEur" type="number"/>

        Stock: <b-input v-model="stock" type="number"/>


      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <button class="btn btn-danger" type="reset" @click="errorMsg = false">RESET</button>
      <button class="btn btn-success" @click.prevent="create">CREATE</button>
      <br><br>
      <div><nuxt-link class="btn btn-info" to="/products">Return</nuxt-link></div>
    </form>
  </div>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                productType:{},
                productTypeId:"",
                productTypes:[],
                description:"",
                valueInEur:0,
                stock:0,
                errorMsg: false
            }
        },
        methods: {
            create() {
                this.$axios.$post('http://localhost:8080/SportsClubManagement_war_exploded/api/products', {
                    productTypeId: this.productTypeId,
                    description: this.description,
                    valueInEur: this.valueInEur,
                    stock: this.stock
                })
                    .then(() => {
                        this.$router.push('/products')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            getProductType(id){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/productTypes/"+id)
                    .then(productType => {
                        this.productType = productType;
                    });
            }
        },
        created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/productTypes/")
                .then(productTypes => {
                    this.productTypes = productTypes;
                });
        }
    }
</script>
