<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input type="text" name="username" v-model="username"/> <br>
    <button @click="getUserData"> Search </button>

    <div v-if="user !== null" class="profile">
      <img v-bind:src="user.avatar_url" width="200" height="200"> <br>
      <label v-if="user.login">Username : {{ user.login }}</label> <br>
      <label v-if="user.name">Profile name : {{ user.name | capitalize }}</label> <br>
      <label v-if="user.bio">Profile bio : {{ user.bio }}</label> <br>
      <label v-if="user.created_at">Account created : {{ user.created_at | formatDate }}</label>
    </div>
  </div>
</template>

<script>
  import GithubService from '@/services/GithubService'
  import moment from 'moment'
  export default {
    name: 'hello',
    data () {
      return {
        msg: 'VueJS PWA consuming Github API',
        username: '',
        user: null
      }
    },
    methods: {
      async getUserData () {
        this.user = null
        await GithubService.searchUser({
          username: this.username
        }).then(response => {
          this.user = response.data
        }).catch(error => {
          console.log(error)
        })
        if (this.user !== null) {
          console.log(this.user)
        }
      }
    },
    filters: {
      formatDate (value) {
        if (value) {
          return moment(String(value)).format('dddd, MMMM Do YYYY - h:mm:ss a')
        }
      },
      capitalize (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
  }
</script>

<style scoped>
  h1, h2 {
    font-weight: normal;
  }
  input {
    width: 40%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }
  button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
  }
  .profile {
    padding: 16px;
  }
  label {
    font-size: 20px;
  }
  
</style>