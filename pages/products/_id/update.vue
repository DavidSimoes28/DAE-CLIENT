private int productTypeId;
private String description;
private Double valueInEur;
private int stock;
<template>
  <div>
    <h1>Update a Product</h1>
    <form @submit.prevent="update">

        Product Type: <b-select v-model="product.productTypeId" :options="productTypes" required value-field="id" text-field='type' v-on:change="getProductType(productTypeId)">
        <template v-slot:first>
          <option :value="null" disabled>-- Please select the athlete --</option>
        </template>
      </b-select>

      Description: <b-input v-model="product.description" type="text"/>


      Value: <b-input v-model="product.valueInEur" type="number"/>


      Stock: <b-input v-model="product.stock" type="number"/>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/products">Return</nuxt-link>
      <button type="reset" @click="errorMsg = false">RESET</button>
      <button @click.prevent="update">Update</button>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                product:{},
                productType:{},
                productTypeId:"",
                productTypes:[],
                description:"",
                valueInEur:0,
                stock:0,
                errorMsg: false
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        methods: {
            update() {
                this.$axios.$put(`http://localhost:8080/SportsClubManagement_war_exploded/api/products/${this.id}`, this.product)
                    .then(() => {
                        this.$router.push('/products')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/products/${this.id}`)
                .then(product => this.product = product || {});
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/productTypes/")
                .then(productTypes => {
                    this.productTypes = productTypes;
                });

        }
    }
</script>

