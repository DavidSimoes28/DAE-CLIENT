<template>
  <div>
    <b-container>
      <h1>Create a new Subscription</h1>
      <form @submit.prevent="create">

        Athlete: <b-select v-model="athleteUsername" :options="athletes" required value-field="id" text-field='username' v-on:change="getAthlete(athleteUsername)">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the athlete --</option>
          </template>
        </b-select>

        <div v-if="athlete.username != undefined">
          <p>Username: {{ athlete.username }}</p>
          <p>Name: {{ athlete.name }}</p>
          <p>Email: {{ athlete.email }}</p>
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

          Graduation: <b-select v-model="graduationId" :options="graduations" required value-field="id" text-field="name" v-on:change="getGraduation(graduationId)">
            <template v-slot:first>
              <option :value="0">-- Select the graduation if is needed --</option>
            </template>
          </b-select>

          <div v-if="graduation.id != undefined || graduation.id == 0">
            <p>Graduation Id: {{ graduation.id }}</p>
            <p>Modality Id: {{ graduation.modalityId}}</p>
          </div>

          State: <b-select v-model="stateId" :options="states" required value-field="id" text-field="name">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the payments --</option>
          </template>
        </b-select>
        </div>
        <div><br>
          Price: <input v-model="price" type="number">
        </div>
        <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
        <nuxt-link to="/subscriptions">Return</nuxt-link>
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
                athletes: [],
                schedules: [],
                states: [],
                echelons: [],
                graduations: [],
                modalityId: [],
                athleteUsername: "",
                scheduleId: 0,
                stateId: 0,
                echelonId: 0,
                graduationId: 0,
                athlete:{},
                modality:{},
                schedule:{},
                echelon:{},
                graduation:{},
                date: null,
                price: 0,
                dayOfWeek:"",
                errorMsg:""
            }
        },
        methods: {
            create() {
                this.$axios.$post("http://localhost:8080/SportsClubManagement_war_exploded/api/subscriptions/",{
                    athleteUsername: this.athleteUsername,
                    echelonId: this.echelonId,
                    graduationId: this.graduationId,
                    modalityId: this.modalityId,
                    scheduleId: this.scheduleId,
                    subscriptionPrice: this.price,
                    stateId: this.stateId
                })
                    .then(athlete => {
                        this.athlete = athlete;
                        this.$router.push('/subscriptions')
                    });
            },
            getAthlete(athleteUsername){
                this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/athletes/"+athleteUsername)
                    .then(athlete => {
                        this.athlete = athlete;
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
            },getGraduation(graduationId){
                if(graduationId != 0)
                {
                  this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/graduations/"+graduationId)
                      .then(graduation => {
                          this.graduation = graduation;
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
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/athletes")
                .then(athletes => {
                    this.athletes = athletes;
                });
            this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/states")
                .then(states => {
                    this.states = states;
                });
        }
    }
</script>
