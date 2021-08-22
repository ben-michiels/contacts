<template>
  <div id="app" class="page-container">
      <component v-bind:is="currentComponent" v-bind="currentProperties" v-on:show-contact="displayUser" v-on:show-list="displayList"></component>
  </div>
</template>

<script>
import ContactList from "./components/ContactList.vue";
import ContactDetails from "./components/ContactDetails.vue";
import axios from "axios";

export default {
  name: 'App',
  components: {
    ContactList,
    ContactDetails
  },
  data: () => ({
    users: [],
    user:{},
    currentComponent: "ContactList",
  }),
  methods:{
    displayUser: function(user){
      this.currentComponent="ContactDetails"
      this.user=user
    },
    displayList: function(){
      this.currentComponent="ContactList"
    }
  },
  computed:{
    currentProperties: function(){
      if(this.currentComponent==="ContactList"){
        return{users:this.users}
      } else if(this.currentComponent==="ContactDetails"){
        return{user:this.user}
      } else {
        return{}
      }
    }
  },
  mounted() {
    axios
      .get("https://randomuser.me/api/?results=50&nat=gb")
      .then((response) => (this.users = response.data.results))
  },
}
</script>