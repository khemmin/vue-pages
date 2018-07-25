<template>
  <div class="hello">
    <div class="holder">

      <form @submit.prevent="addSkill">
        <div class="ui icon input">
        <input type="text" placeholder="Enter a skill you have.." v-model="skill" v-validate="'min:5'" name="skill">
        <transition name="alert-in" enter-active-class="animated flipInX" leave-active-class="animated filpOutX">
          <p class="alert" v-if="errors.has('skill')">{{ errors.first('skill') }}</p>
        </transition>
        <i class="inverted circular search link icon"></i>
        </div>
        <div>
          <!-- <date-picker v-model="time1" :first-day-of-week="1" lang ="en"></date-picker> -->
          <date-picker v-model="time2" type="date" :time-picker-options="timePickerOptions" lang="en"></date-picker>
          <date-picker v-model="time3" range :shortcuts="shortcuts" lang="en"></date-picker>
          <p v-if="time2 != null">
          <!-- {{ time2.getDate() }}
          {{ time2.getMonth() }}
          {{ time2.getFullYear() }} -->
          </p>
        </div>
      </form>
      
      <ul>
        <transition-group name="list" enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
          <li v-for="(data, index) in skills" :key='index'>
            {{data.skill}}
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
          </li>

        </transition-group>
      </ul>

    <p>These are the skills that you possess. </p>

    <table class="ui celled structured table">
      <thead>
        <tr>
          <th rowspan="2">Name</th>
          <th rowspan="2">Type</th>
          <th rowspan="2">Files</th>
          <th colspan="3">Languages</th>
        </tr>
        <tr>
          <th>Ruby</th>
          <th>JavaScript</th>
          <th>Python</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Alpha Team</td>
          <td>Project 1</td>
          <td class="right aligned">2</td>
          <td class="center aligned">
            <i class="large green checkmark icon"></i>
          </td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td rowspan="3">Beta Team</td>
          <td>Project 1</td>
          <td class="right aligned">52</td>
          <td class="center aligned">
            <i class="large green checkmark icon"></i>
          </td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>Project 2</td>
          <td class="right aligned">12</td>
          <td></td>
          <td class="center aligned">
            <i class="large green checkmark icon"></i>
          </td>
          <td></td>
        </tr>
        <tr>
          <td>Project 3</td>
          <td class="right aligned">21</td>
          <td class="center aligned">
            <i class="large green checkmark icon"></i>
          </td>
          <td></td>
          <td></td>
        </tr>
      </tbody>
    </table>
    </div>
  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker'

export default {
  name: 'Skills',
  components: { DatePicker },
  data(){
    return {
      skill: '',
      skills: [
        { "skill": "Vue.js"},
        { "skill": "Frontend Developer"}
      ],
      time1: '',
      time2: '',
      time3: '',
      time4: '',
      shortcuts: [
        {
          text: 'Today',
          onClick: () => {
            this.time3 = [ new Date(), new Date() ]
          }
        }
      ],
      timePickerOptions:{
        start: '00:00',
        step: '00:30',
        end: '23:30'
      }
    }
  },
  methods: {
    addSkill(){
      this.$validator.validateAll().then((result) => {
          if (result) {
            this.skills.push({skill: this.skill});
            this.skill = '';
          } else {
            console.log('Not valid');
          }
        })
    },
    remove(id) {
      this.skills.splice(id,1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

@import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

.holder {
    background: #fff;
  }

  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  
  ul li {
    padding: 20px;
    font-size: 1.3em;
    background-color: #E0EDF4;
    border-left: 5px solid #3EB3F6;
    margin-bottom: 2px;
    color: #3E5252;
  }

  p {
    text-align:center;
    padding: 30px 0;
    color: gray;
  }

  .container {  
    box-shadow: 0px 0px 40px lightgray;
  }

  /* input {
    width: calc(100% - 40px);
    border: 0;
    padding: 20px;
    font-size: 1.3em;
    background-color: #323333;
    color: #687F7F;
  } */

  .alert {
    background: #fdf2ce;
    font-weight: bold;
    display: inline-block;
    padding: 5px;
    margin-top: -20px;
  }

  .alert-in-enter-active {
  animation: bounce-in .5s;
  }
  .alert-in-leave-active {
    animation: bounce-in .5s reverse;
  }
  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }


</style>

<script>

import axios from 'axios'

export default {
  name : 'timeAttendance',
  data(){
    return{
      JournDatabase : [],
      JournCCureDatabase : []
    }
  },
  methods:{
    getJournDatabase(empcode,year,month,date){
      axios.get("http://192.9.59.123:8081/allFromJourn/"+empcode+"/"+year+"/"+month+"/"+date)
      .then(response => {
        this.JournDatabase = response.data;
        console.log(this.JournDatabase)
      })
      .catch(error =>{
        console.log(error);
      })
    }
    ,
    getJournCCureDatabase(empcode,year,month,date){
      axios.get("http://192.9.59.123:8081/allFromJournCCure/"+empcode+"/"+year+"/"+month+"/"+date)
      .then(response =>{
        this.JournCCureDatabase = response;
        console.log(this.JournCCure)
      })
      .catch(error){
        console.log(error);
      }
    }
  }
}
</script>

