<template>
  <div>
    <form @submit.prevent="login">
        <div class="mb-3">
            <label for="exampleInputEmail1" class="form-label">Email address</label>
            <input type="email" v-model="user" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
            <div id="emailHelp"  class="form-text">We'll never share your email with anyone else.</div>
        </div>
        <div class="mb-3">
            <label for="exampleInputPassword1" class="form-label">Password</label>
            <input type="password" v-model="password" class="form-control" id="exampleInputPassword1">
        </div>        
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
const axios = require('axios');
export default {
    data() {
        return {
            user:[],
            password:[]            
        }
    },
    mounted () {
        
           
    },
    methods: {
        login() {
             axios.post('https://dev-api.txbd.win/api/user/login', {
                user: this.user,
                password: this.password
            })
            .then((response)=> {    
                localStorage.setItem('token',response.data.auth_token)        
                this.$router.push({name:'Home'});            
            })
            .catch(function (error) {
            console.log(error);
            });
        
            
        }
    },

}
</script>

<style>

</style>