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
              <input type="text" id="search" v-on:keyup="filterList()">
            </md-field>
          </div>
          <div id="sorting" class="md-toolbar-row md-layout md-alignment-center-center">
           <p class="md-layout-item md-size-10 sorting-label">Sort</p>
            <md-field md-align-trigger class="md-layout-item md-size-75 sorting-bar" id="sort-bar">
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
            <div v-for="user of filteredUsers" v-bind:key="users[user].login.uuid">
              <contact-item v-bind:user="users[user]"></contact-item>
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

function filterList() {
  let newFilteredUsers = []
  let searchContents = document.getElementById("search").value
  if (searchContents == "") {
    this.filteredUsers = Array.from(this.users.keys())
    return
  }
  Array.from(this.users).forEach((user,index)=>{
    let testStrings = [user.name.first, user.name.last, user.location.country, user.location.state]
    let searchContentsFound = false
    testStrings.forEach(string => {
      if(string.toUpperCase().includes(searchContents.toUpperCase())){
        searchContentsFound = true
      }
    })
    if(searchContentsFound){
      newFilteredUsers.push(index)
    }
  })
  this.filteredUsers = newFilteredUsers
}

export default {
  name: 'App',
  components: {
    contactItem
  },
  methods: {
    filterList
  },
  data: () => ({
    sort: "first name",
    users: [],
    filteredUsers: [],
  }),
  mounted() {
    axios
      .get("https://randomuser.me/api/?results=50&nat=gb")
      .then((response) => (this.users = response.data.results)).then(filterList.bind(this));
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
#search {
  background-image: url('assets/1x/outline_search_black_24dp.png'); /* Add a search icon to input */
  background-position: 10px 12px; /* Position the search icon */
  background-repeat: no-repeat; /* Do not repeat the icon image */
  background-color: white;
  width: 100%; /* Full-width */
  font-size: 16px; /* Increase font-size */
  padding: 12px 20px 12px 40px; /* Add some padding */
  border: 1px solid #ddd; /* Add a grey border */
  margin-bottom: 12px; /* Add some space below the input */
}
.sorting-bar {
  padding:0.5em;
}
#sort-bar {
  background-color: white;
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
