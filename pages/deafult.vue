<template>
  <div>
    <div>
      <b-form @submit.prevent="logout">
        <b-button type="Submit" class="btn-success">Logout</b-button>
      </b-form>
    </div>
  <nuxt/>
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
