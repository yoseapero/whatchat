<template>
  <f7-app :params="f7params" >

  <!-- Left panel with cover effect-->
  <f7-panel left cover theme-dark>
    <f7-view>
      <f7-page>
        <div class="wrapper">
        <img class="image--cover" :src="photo_url" alt="">
        </div>
        <f7-block style="text-align:center">{{display_name}}</f7-block>
        
        
        <f7-list>
          <f7-list-item link="/signin/" view=".view-main" v-if="!signed_in" panel-close title="Sign In"></f7-list-item>
          <f7-list-item @click="signOut" view=".view-main" panel-close title="Sign Out"></f7-list-item>
        </f7-list>
      </f7-page>
    </f7-view>
  </f7-panel>


  <!-- Right panel with reveal effect-->
  <f7-panel right reveal theme-dark>
    <f7-view>
      <f7-page>
        <f7-navbar title="Right Panel"></f7-navbar>
        <f7-block>Right panel content goes here</f7-block>
      </f7-page>
    </f7-view>
  </f7-panel>


  <!-- Views/Tabs container -->
  <f7-views tabs class="safe-areas" v-if="signed_in">
    <!-- Tabbar for switching views-tabs -->
    <f7-toolbar tabbar labels bottom>
      <f7-link
      tab-link="#view-home" 
      tab-link-active 
      icon-ios="f7:house_fill" 
      icon-aurora="f7:house_fill" 
      icon-md="material:home" 
      text="Home">
      </f7-link>
      
      <f7-link 
      tab-link="#view-catalog" 
      icon-ios="f7:square_list_fill" 
      icon-aurora="f7:square_list_fill" 
      icon-md="material:view_list" 
      text="Catalog">
      </f7-link>
      
      <f7-link 
      tab-link="#view-settings" 
      icon-ios="f7:gear" 
      icon-aurora="f7:gear" 
      icon-md="material:settings" 
      text="Edit Profile">
      </f7-link>
    </f7-toolbar>

    <!-- Your main view/tab, should have "view-main" class. It also has "tab-active" class -->
    <f7-view id="view-home" main tab tab-active url="/"></f7-view>

    <!-- Catalog View -->
    <f7-view id="view-catalog" name="catalog" tab url="/catalog/"></f7-view>

    <!-- Settings View -->
    <f7-view id="view-settings" name="editprofile" tab url="/editprofile/"></f7-view>

  </f7-views>

  <f7-view v-if="!signed_in" url="/signin/" :main="true">

  </f7-view>


    <!-- Popup -->
    <f7-popup id="my-popup">
      <f7-view>
        <f7-page>
          <f7-navbar title="Popup">
            <f7-nav-right>
              <f7-link popup-close>Close</f7-link>
            </f7-nav-right>
          </f7-navbar>
          <f7-block>
            <p>Popup content goes here.</p>
          </f7-block>
        </f7-page>
      </f7-view>
    </f7-popup>

    <f7-login-screen id="my-login-screen">
      <f7-view>
        <f7-page login-screen>
          <f7-login-screen-title>Login</f7-login-screen-title>
          <f7-list form>
            <f7-list-input
              type="text"
              name="username"
              placeholder="Your username"
              :value="username"
              @input="username = $event.target.value"
            ></f7-list-input>
            <f7-list-input
              type="password"
              name="password"
              placeholder="Your password"
              :value="password"
              @input="password = $event.target.value"
            ></f7-list-input>
          </f7-list>
          <f7-list>
            <f7-list-button title="Sign In" @click="alertLoginData"></f7-list-button>
            <f7-block-footer>
              Some text about login information.<br>Click "Sign In" to close Login Screen
            </f7-block-footer>
          </f7-list>
        </f7-page>
      </f7-view>
    </f7-login-screen>
  </f7-app>
</template>
<script>
  import firebase from 'firebase';
  import { Device }  from 'framework7/framework7-lite.esm.bundle.js';
  import cordovaApp from '../js/cordova-app.js';
  import routes from '../js/routes.js';
import { mixin } from '../js/mixin.js';

  var firebaseConfig = {
    apiKey: "AIzaSyD3_ZP0QS-m7I50DYNvthR88ced6LrEbu8",
    databaseURL: "https://whatchat-fcbb8-default-rtdb.firebaseio.com",
    authDomain: "whatchat-fcbb8.firebaseapp.com",
    projectId: "whatchat-fcbb8",
    storageBucket: "whatchat-fcbb8.appspot.com",
    messagingSenderId: "632445008476",
    appId: "1:632445008476:web:ee2d8ccfb06d1d6c57c499",
    measurementId: "G-JSR8QDCC15"
  }; 
  // Initialize Firebase
  firebase.initializeApp(firebaseConfig);
  //firebase.analytics();

  export default {
    data() {
      return {
        // Framework7 Parameters
        f7params: {
          id: 'io.framework7.myapp', // App bundle ID
          name: 'WhatChat', // App name
          theme: 'auto', // Automatic theme detection

          // App root data
          data: function () {
            return {

              // Demo products for Catalog section
              products: [
                {
                  id: '1',
                  title: 'Apple iPhone 8',
                  description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Nisi tempora similique reiciendis, error nesciunt vero, blanditiis pariatur dolor, minima sed sapiente rerum, dolorem corrupti hic modi praesentium unde saepe perspiciatis.'
                },
                {
                  id: '2',
                  title: 'Apple iPhone 8 Plus',
                  description: 'Velit odit autem modi saepe ratione totam minus, aperiam, labore quia provident temporibus quasi est ut aliquid blanditiis beatae suscipit odio vel! Nostrum porro sunt sint eveniet maiores, dolorem itaque!'
                },
                {
                  id: '3',
                  title: 'Apple iPhone X',
                  description: 'Expedita sequi perferendis quod illum pariatur aliquam, alias laboriosam! Vero blanditiis placeat, mollitia necessitatibus reprehenderit. Labore dolores amet quos, accusamus earum asperiores officiis assumenda optio architecto quia neque, quae eum.'
                },
              ]
            };
          },

          // App routes
          routes: routes,


          // Input settings
          input: {
            scrollIntoViewOnFocus: Device.cordova && !Device.electron,
            scrollIntoViewCentered: Device.cordova && !Device.electron,
          },
          // Cordova Statusbar settings
          statusbar: {
            iosOverlaysWebView: true,
            androidOverlaysWebView: false,
          },
        },
        // Login screen data
        username: '',
        password: '',
      }
    },
    mixins: [mixin],
    computed:{
      signed_in(){
        return this.$store.getters.signed_in
      },
      photo_url(){
        return this.$store.getters.photo_url
      },
      display_name(){
        return this.$store.getters.display_name
      }
    },
    methods: {
      alertLoginData() {
        this.$f7.dialog.alert('Username: ' + this.username + '<br>Password: ' + this.password, () => {
          this.$f7.loginScreen.close();
        });
      
    },
    signOut(){
      const app = this.$f7
      this.$store.dispatch('signOut')
      app.panel.close()
    }
    },
    mounted() {
      this.$f7ready((f7) => {
        // Init cordova APIs (see cordova-app.js)
        if (Device.cordova) {
          cordovaApp.init(f7);
        }
        // Call F7 APIs here
      });
    }
  }
</script>

<style scoped>
.wrapper {
  text-align: center;
}
.image--cover {
  width: 100x;
  height: 100px;
  border-radius: 60%;
  margin: 10px;
  object-fit: cover;
  object-position: center;
}
</style>