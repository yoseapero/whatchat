<template>
  <f7-page name="signin">
    <f7-navbar title="Sign In" back-link="Back"></f7-navbar>
    
    
   <f7-list no-hairlines-md>
  
  <f7-list-input :value="email" @input="email=$event.target.value" label="E-mail" type="email" placeholder="Your e-mail" clear-button
  >
    
  </f7-list-input>
  
  <f7-list-input :value="password" @input="password=$event.target.value" label="Password" type="password" placeholder="Your password" clear-button
  >
    
  </f7-list-input>



  </f7-list>

  <f7-block>

  <f7-button fill round @click="signIn">Login</f7-button>
  <div style="text-align:center">
      
  <f7-link v-if="show_resend_email" @click="resendEmail" :color="color(time_left)">Resend Confirmation Email <span v-if="time_left>0">&nbsp; {{time_left}}</span></f7-link> <br>
  <f7-link href="/signup/"> Don't have an account? Sign Up</f7-link> <br>
  <f7-link @click="forgetPassword" >Forgot Password</f7-link>
  </div>
  </f7-block>
  
  </f7-page>
</template>

<script>
import {setInterval, clearInterval} from 'timers';
import firebase from 'firebase';
import {mixin} from "../../js/mixin";
export default {

  mixins: [mixin],
    data(){
        return{
            email:null,
            password:null,
            time_left:-1
        }
    },
    computed:{
      show_resend_email(){
        return this.$store.getters.show_resend_email
      }
    },
    methods:{
      forgetPassword(){
        console.log('forgetPassword');
        const self = this
        var auth = firebase.auth();

        if(this.email != null){
          auth.sendPasswordResetEmail(this.email).then(function () {
          
          self.$store.commit('setAlertMessage', 'An reset email has been sent')
        
        }).catch(function (error) {
        
          self.$store.commit('setAlertMessage', error)
        
        });

        }else{
          self.$store.commit('setAlertMessage', 'Please enter your email')
        }
        
        
      },
      color(timeleft){
            if (timeleft <= 0){
            return '#007aff'
            }else{
              return 'gray'
            }
      },
      resendEmail(){
        const self = this
        
        if(this.time_left<=0){
          this.$store.dispatch('sendVerification')
          self.countDown()
        }
        
      },
      countDown(){
        const self = this
        self.time_left = 20
        var timer = setInterval(function(){
          self.time_left -= 1
          console.log('time_left', self.time_left)
          if(self.time_left<= 0){
            clearInterval(timer)
          }
        }, 1000)
      },
      signIn(){
       var payload = {}
       payload.email = this.email,
       payload.password = this.password
       this.$store.dispatch('signIn',payload)
      }
    }
}
</script>