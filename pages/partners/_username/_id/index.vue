<template>
  <b-container>
    <h4>Upload receipt of payment</h4>

    <form @submit.prevent="upload">
      <b-form-file v-model="file" :state="Boolean(file)" placeholder="Choose a file or drop it here..." drop-placeholder="Drop file here..." />
      <button @click.prevent="upload">UPLOAD</button>
    </form>
    <nuxt-link :to="`/partners/${username}`">Back</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                payment: {},
                purchase: {},
                file: null
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            },
            username() {
                return this.$route.params.username
            }
        },methods: {
            upload() {
                if(this.file != null){
                  let formData = new FormData();

                  formData.append('file', this.file);

                  this.$axios.$post(`/api/file/${this.purchase.id}/payment/${this.payment.id}/upload`, formData, {
                      headers: {
                          'Content-Type': 'multipart/form-data'
                      }
                  })
                      .then(() => {
                          this.$toast.success('file sent');
                          this.$router.push(`/partners/${this.username}`);
                      })
                      .catch(error => {
                          this.$toast.error('error sending the file')
                      })
              }
              else{
                    this.$toast.error('Chose a file!')
              }
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/payments/${this.id}`)
                .then(payment =>{
                    this.payment = payment || {};
                    this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/purchases/${this.payment.purchaseId}`)
                        .then(purchase =>{
                            this.purchase = purchase || {};
                        });
                });
        },
    }
</script>


