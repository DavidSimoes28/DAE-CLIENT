<template>
  <b-container>
    <b-form @submit.prevent="logout">
      <b-button type="Submit" class="btn-success">Logout</b-button>
    </b-form>
    <h1>Welcome to Sports Club Management</h1>
    Please visit our
    <nuxt-link to="/administrators" class="btn btn-link">Administrator</nuxt-link>
    <nuxt-link to="/modalities" class="btn btn-link">Modality</nuxt-link>
    <nuxt-link to="/athletes" class="btn btn-link">Athletes</nuxt-link>
    <nuxt-link to="/partners" class="btn btn-link">Partners</nuxt-link>
    <nuxt-link to="/subscriptions" class="btn btn-link">Subscriptions</nuxt-link>
    <nuxt-link to="/classes" class="btn btn-link">Classes</nuxt-link>
  </b-container>
</template>
<script>
    export default {
        methods: {
            logout(){
                this.$auth.logout('local');
                this.$router.push("auth/login");
            }
        }
    }
</script>

