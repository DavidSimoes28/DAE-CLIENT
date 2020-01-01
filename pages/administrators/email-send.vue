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

      <div>
        Modality: <b-select v-model="modalityId" :options="modalities" required value-field="id" text-field="name">
        <template v-slot:first>
          <option :value="0" selected>None</option>
        </template>
      </b-select>
      </div>

      <div>
        Echelon: <b-select v-model="echelonId" :options="echelons" required value-field="id" text-field="name">
        <template v-slot:first>
          <option :value="0" selected>None</option>
        </template>
      </b-select>
      </div>

      <div>
        <button class="btn btn-link" v-on:click.prevent="filter">Filter</button>
        <button class="btn btn-link" v-on:click.prevent="filterReset">Reset Filters</button>
      </div>

      <b-table striped over :items="users" :fields="fields"/>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                users: [],
                modalityId:"0",
                echelonId:"0",
                echelons:[],
                modalities:[],
                fields: ['username', 'name', 'email'],
                subject: null,
                message: null,
            }
        },
        created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/email/send/users")
                .then(users => {
                    this.users = users;
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities")
                .then(modalities => this.modalities = modalities);
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/echelons")
                .then(echelons => this.echelons = echelons);
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
            },
            filter(){
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/email/send/filter", {
                    modalityId: this.modalityId,
                    echelonId: this.echelonId
                }).then(users => this.users = users);
            },
            filterReset(){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/email/send/users")
                    .then(users => {
                        this.users = users;
                    });
            }
        }
    }
</script>
