<template>
  <f7-page name="editprofile">
    <f7-navbar title="Edit Profile"></f7-navbar>
    <div class="wrapper">
      <img
        class="image--cover"
        :src="image_url"
        alt=""
        @click="launchFilePicker"
      />
    </div>

    <f7-list no-hairlines-md>
      <f7-list-input :value="display_name" @input="display_name = $event.target.value" label="Name" type="text" placeholder="Your name" clear-button >
      
      </f7-list-input>

    </f7-list>

    <f7-block>
      <f7-button fill round >Update</f7-button>
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
    };
  },
  computed: {

    display_name:{
        get:function(){
            return this.$store.getters.display_name
        },
        set:function(newValue){
            this.$store.commit('setDisplayName', newValue)
        }
    },
    image_url() {
      return this.$store.getters.image_url
    },
    files() {
      return this.$store.getters.files
    },
    photo_url(){
        return this.$store.getters.photo_url
    }
  },
  watch: {
    
  },

  methods: {
    launchFilePicker() {
      this.$refs.file.click();
    },
    onFilePicked() {
      //read the image file
      this.$store.dispatch("readFile");
    },
    
  },
  created() {
    if(this.photo_url!= null){
        this.$store.commit('setImageURL',this.photo_url)
    }
  }
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