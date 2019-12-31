<template>
  <b-container>
    <h4>Upload receipt of payment</h4>
    <p>{{payment.id}}</p>
    <p>{{payment.purchaseId}}</p>

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
                paymentField: ['id', 'purchaseId',"stateId","actions"],
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

                  this.$axios.$post(`/api/file/${this.payment.id}/upload`, formData, {
                      headers: {
                          'Content-Type': 'multipart/form-data'
                      }
                  })
                      .then(() => {
                          this.$toast.success('file sent')
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
                .then(payment => this.payment = payment || {});
        },
    }
</script>


