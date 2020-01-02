<template>
  <b-container>
    <form @submit.prevent="send">
      <div>
        Subject:
        <b-input v-model="subject" type="text"/>
      </div>
      <div>
        Message:<b-textarea v-model="message" name="message"></b-textarea>
      </div>
      <nuxt-link :to="`/coaches/${this.username}`">Back</nuxt-link>
      &nbsp
      <button class="btn btn-success" @click.prevent="send">SEND</button>

      <br><br>
      <h4>{{this.username}}'s Athletes</h4>
      <b-table striped over :items="users" :fields="fields"/>
    </form>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                users: [],
                fields: ['username', 'name', 'email'],
                subject: null,
                message: null,
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        methods: {
            send() {
                this.$axios.$post(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/email/send`, {
                    subject: this.subject,
                    message: this.message,
                    users: this.users
                }).then(msg => {
                    this.$toast.success(msg)
                }).catch(error => {
                    this.$toast.error('error sending the e-mail')
                })
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/${this.username}/athletes`)
                .then(users => {
                    this.users = users;
                });
        }
    }
</script>
