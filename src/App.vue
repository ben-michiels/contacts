<template>
  <div id="app" class="page-container"> 
    
    <md-app md-waterfall md-mode="fixed">
      <md-app-toolbar md-theme="default-dark">
        <div class="md-toolbar-row md-layout md-alignment-bottom-left">
          <md-icon class="md-layout-item md-size-10">people</md-icon>
          <span class="md-title md-layout-item">My Contacts</span>
        </div>
        
        <div id="list-controls">
          <div id="searching" class="md-toolbar-row md-layout md-alignment-center-center">
            <p class="md-layout-item md-size-10 sorting-label">Search</p>
            <md-field md-inline class="md-layout-item md-size-75 sorting-bar">
              <md-input v-model="inline" md-layout="box" md-dense></md-input>
            </md-field>
          </div>
          <div id="sorting" class="md-toolbar-row md-layout md-alignment-center-center">
           <p class="md-layout-item md-size-10 sorting-label">Sort</p>
            <md-field md-align-trigger class="md-layout-item md-size-75 sorting-bar">
             <md-select v-model="sort" name="sort" id="sort">
               <md-option value="first-name">First Name</md-option>
               <md-option value="surname">Surname</md-option>
               <md-option value="country">Country</md-option>
             </md-select>
            
           </md-field>
          </div>
        </div>
      </md-app-toolbar>

        <md-app-content>
        <div id="scrolling-list">
          <md-list class="md-double-line">
            <!-- input cards here -->
            <div v-for="user of users" v-bind:key="user.login.uuid">
              <contact-item v-bind:user="user"></contact-item>
            </div>
          </md-list>                       
      </div>
      </md-app-content>
    </md-app>
  </div>
</template>

<script>
import contactItem from './components/contact-item.vue'
import axios from "axios";

export default {
  name: 'App',
  components: {
    contactItem
  },
  data: () => ({
    sort: 'first-name',
    users: [],
  }),
  mounted() {
    axios
      .get("https://randomuser.me/api/?results=50&nat=gb")
      .then((response) => (this.users = response.data.results));
  },
}
</script>

<style>
#app {
  font-family: Roboto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
  margin-top: 0px;
}
.md-app{
  height:100vh
}
.md-app-content {
  padding:0;
}
.md-icon {
  margin: 0em;
  margin-left: 1em;
  color:white;
}
.md-title {
  margin-top: 1.1em;
  color:white;
}
.md-app-toolbar {
  gap: 1em;
  min-height: 5em;
  background-color: #212121;
  padding:0;
}
.md-toolbar-row {
  align-content: flex-end;
}
#list-controls {
  padding-top: 1em;
  background-color: #f4f4f4;
  min-width: 100%;
}
#searching,#sorting {
  margin-left:0;
  max-width: 100%;
  gap: 10%;
}
.sorting-bar {
  background-color: white;
  padding:0.5em;
}
.sorting-label {
  padding-bottom:1.2em;
}
#scrolling-list {
  background-color:white;
  min-height:100%;
}
.md-list {
  padding:0;
}
</style>
