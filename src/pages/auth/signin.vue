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
      
  <f7-link @click="resendEmail">Resend Confirmation Email <span v-if="time_left>0"></span></f7-link> <br>
  <f7-link href="/signup/"> Don't have an account? Sign Up</f7-link> <br>
  <f7-link>Forgot Password</f7-link>
  {{time_left}}
  </div>
  </f7-block>
  
  </f7-page>
</template>

<script>
import {setInterval, clearInterval} from 'timers';
export default {
    data(){
        return{
            email:null,
            password:null,
            time_left:-1
        }
    },
    methods:{
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