<template>
  <f7-page name="signup">
    <f7-navbar title="Sign Up" back-link="Back"></f7-navbar>
    <div class="wrapper">
      <img
        class="image--cover"
        :src="image_url"
        alt=""
        @click="launchFilePicker"
      />
    </div>

    <f7-list no-hairlines-md>
      <f7-list-input
        :value="name"
        @input="name = $event.target.value"
        label="Name"
        type="text"
        placeholder="Your name"
        clear-button
      >
      </f7-list-input>

      <f7-list-input
        :value="email"
        @input="email = $event.target.value"
        label="E-mail"
        type="email"
        placeholder="Your e-mail"
        clear-button
      >
      </f7-list-input>

      <f7-list-input
        :value="password"
        @input="password = $event.target.value"
        label="Password"
        type="password"
        placeholder="Your password"
        clear-button
      >
      </f7-list-input>
    </f7-list>

    <f7-block>
      <f7-button fill round @click="signUp">Register</f7-button>
      <input
        type="file"
        ref="file"
        style="display: none"
        @change="onFilePicked"
      />
    </f7-block>
  </f7-page>
</template>

<script>
// import {
//   setTimeout
// } from 'timers';
import { mixin } from "../../js/mixin";
export default {
  mixins: [mixin],
  data() {
    return {
      name: null,
      email: null,
      password: null,
    }
  },
  computed: {
    image_url() {
      return this.$store.getters.image_url;
    }
  },

  methods: {
    launchFilePicker() {
      this.$refs.file.click();
    },
    onFilePicked() {
      //read the image file
      this.$store.dispatch('readFile')
    },
    signUp() {
      const self = this;
      var payload = {};
      payload.name = this.name;
      payload.email = this.email;
      payload.password = this.password;
      payload.photoURL = this.image_url;
      //alert(JSON.stringify(payload))
      this.$store.dispatch("signUp", payload);
    },
  },
};
</script>
<style scoped>
.wrapper {
  text-align: center;
}
.image--cover {
  width: 150px;
  height: 150px;
  border-radius: 60%;
  margin: 20px;
  object-fit: cover;
  object-position: center;
}
</style>