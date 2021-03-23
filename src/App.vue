<template>
  <v-app>
    <layout v-if="loggedIn"></layout>
      <login v-else></login>
  </v-app>
</template>

<script>
import Layout from './components/Layout';
import Login from './components/auth/login/Login';
import jwtDecode from "jwt-decode";
export default {
  name: 'App',

  components: {
    Layout,
    Login
  },
  data (){
    return {
      loggedIn: true
    }
  },
  created() { 
   	console.log(this.loggedIn)
  },
  mounted() {
    console.log(this.loggedIn)
    console.log("here");
		if (typeof localStorage.getItem("session") != "undefined") {
      var session = JSON.parse(localStorage.getItem("session"));
			if ((session != null  && typeof session.user.access_key != "undefined" && typeof session.user.secret_key != "undefined"  && typeof session.user.endpoint != "undefined")) {
        this.loggedIn = true;
				//console.log(session.user.username);
				// $(".users-dropdown").text(session.user.username);
			}        
    }else {
      this.loggedIn = false
    }
  },
};
</script>
