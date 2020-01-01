<template>
  <div>
    <b-container>
      <b-form @submit.prevent="logout">
        <b-button type="Submit" class="btn-success">Logout</b-button>
      </b-form>
    </b-container>
    <nuxt />
  </div>
</template>
<script>
    export default {
        methods: {
            logout(){
                this.$auth.logout('local');
                this.$router.push("auth/login");
            }
        }
    };
</script>
