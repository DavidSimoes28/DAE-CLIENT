<template>
  <div>
    <form @submit.prevent="send">
      <div>
        subject:
        <input v-model="subject" type="text">
      </div>
      <div>
        message:<textarea v-model="message" name="message"></textarea>
      </div>
      <nuxt-link :to="`/administrators`">Go to Administrators</nuxt-link>
      &nbsp;
      <button @click.prevent="send">SEND</button>

      <b-table striped over :items="coaches" :fields="fields"/>
      <b-table striped over :items="athletes" :fields="fields"/>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                coaches: [],
                athletes:[],
                fields: ['username', 'name', 'email'],
                subject: null,
                message: null,
            }
        },
        created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches")
                .then(coaches => {
                    this.coaches = coaches;
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/athletes")
                .then(athletes => {
                    this.athletes = athletes;
                });
        },
        methods: {
            send() {
                this.$axios.$post(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/email/send`, {
                    subject: this.subject,
                    message: this.message,
                    users: this.coaches
                }).then(msg => {
                        this.$axios.$post(`http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/email/send`, {
                            subject: this.subject,
                            message: this.message,
                            users: this.athletes
                        }).then(msg => {
                            this.$toast.success(msg)
                        });
                    })
                    .catch(error => {
                        this.$toast.error('error sending the e-mail')
                    })
            }
        }
    }
</script>
