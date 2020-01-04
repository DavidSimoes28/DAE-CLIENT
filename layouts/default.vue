<template>
  <div>

      <b-form @submit.prevent="logout">
        <b-button type="Submit" class="btn btn-dark">Logout</b-button>
      </b-form>
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
