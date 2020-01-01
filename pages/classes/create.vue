<template>
  <div>
    <b-container>
      <h1>Create a new Subscription</h1>
      <form @submit.prevent="create">

        Coach: <b-select v-model="coachUsername" :options="coaches" required value-field="id" text-field='username' v-on:change="getCoach(coachUsername)">
        <template v-slot:first>
          <option :value="null" disabled>-- Please select the coach --</option>
        </template>
      </b-select>

        <div v-if="coach.username != undefined">
          <p>Username: {{ coach.username }}</p>
          <p>Name: {{ coach.name }}</p>
          <p>Email: {{ coach.email }}</p>
        </div>

        Modality: <b-select v-model="modalityId" :options="modalities" required value-field="id" text-field="name" v-on:change="getModality(modalityId)">
        <template v-slot:first>
          <option :value="null" disabled>-- Please select the modality --</option>
        </template>
      </b-select>
        <div v-if="modality.id != undefined">
          <p>Modality Id: {{ modality.id }}</p>
        </div>
        <div v-if="modality.id != undefined">
          Schedule: <b-select v-model="scheduleId" :options="schedules" required value-field="id" text-field="id" v-on:change="getSchedule(scheduleId)">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the schedule --</option>
          </template>
        </b-select>

          <div v-if="schedule.startDate != undefined">
            <p>Day of the week: {{ dayOfWeek}}</p>
            <p>Start time: {{ schedule.startDate.hour+":"+schedule.startDate.minutes}}</p>
            <p>End time: {{ schedule.endDate.hour+":"+schedule.endDate.minutes}}</p>
          </div>

          Echelon: <b-select v-model="echelonId" :options="echelons" required value-field="id" text-field="name" v-on:change="getEchelon(echelonId)">
          <template v-slot:first>
            <option :value="0">-- Select the echelon if is needed --</option>
          </template>
        </b-select>

          <div v-if="echelon.id != undefined || echelon.id == 0">
            <p>Echelon Id: {{ echelon.id }}</p>
            <p>Modality Id: {{ echelon.modalityId}}</p>
          </div>
        </div>
        <div><br>
          Date: <input v-model="date" type="date">
        </div>
        <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
        <nuxt-link to="/classes">Return</nuxt-link>
        <button type="reset" @click="errorMsg = false">RESET</button>
        <button @click.prevent="create">CREATE</button>
      </form>
    </b-container>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                modalities: [],
                coaches: [],
                schedules: [],
                echelons: [],
                modalityId: [],
                coachUsername: "",
                scheduleId: 0,
                echelonId: 0,
                coach:{},
                modality:{},
                schedule:{},
                echelon:{},
                graduation:{},
                date: null,
                dayOfWeek:"",
                errorMsg:""
            }
        },
        methods: {
            create() {
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/classes/",{
                    coachUsername: this.coachUsername,
                    echelonId: this.echelonId,
                    modalityID: this.modalityId,
                    scheduleID: this.scheduleId,
                    date: this.date
                }).then(coach => {
                        this.$router.push('/classes')
                    });
            },
            getCoach(username){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches/"+username)
                    .then(coach => {
                        this.coach = coach;
                    });
            },getModality(modalityId){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities/"+modalityId)
                    .then(modality => {
                        this.modality = modality;
                        this.schedules = modality.schedules;
                        this.echelons = modality.echelons;
                        this.graduations = modality.graduations;
                    });
            },getSchedule(scheduleId){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/schedules/"+scheduleId)
                    .then(schedule => {
                        this.schedule = schedule;
                        if(this.schedule.dayOfWeek == 0){ this.dayOfWeek="MONDAY"}
                        if(this.schedule.dayOfWeek == 1){ this.dayOfWeek="TUESDAY"}
                        if(this.schedule.dayOfWeek == 2){ this.dayOfWeek="WEDNESDAY"}
                        if(this.schedule.dayOfWeek == 3){ this.dayOfWeek="THURSDAY"}
                        if(this.schedule.dayOfWeek == 4){ this.dayOfWeek="FRIDAY"}
                        if(this.schedule.dayOfWeek == 5){ this.dayOfWeek="SATURDAY"}
                        if(this.schedule.dayOfWeek == 6){ this.dayOfWeek="SUNDAY"}
                    });
            },getEchelon(echelonId){
                if(echelonId != 0)
                {
                    this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/echelons/"+echelonId)
                        .then(echelon => {
                            this.echelon = echelon;
                        });
                }
            }
        },created() {
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/modalities")
                .then(modalities => {
                    this.modalities = modalities
                })
                .catch(error => {
                    this.errorMsg = error.response.data
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/coaches")
                .then(coaches => {
                    this.coaches = coaches;
                });
        }
    }
</script>
