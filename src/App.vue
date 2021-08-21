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
            <form name="form1" id="form1" class="md-layout-item md-size-75 sorting-bar">
              <select name="sort" id="sort" v-on:change="sortList()">
                <option value="first" selected="selected">First Name</option>
                <option value="last">Last Name</option>
                <option value="state">County</option>
                <option value="country">Country</option>
              </select>
            </form>
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
    this.sortList()
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
  this.sortList()
}

function sortList() {
  let sortContents = document.getElementById("sort").value
  this.filteredUsers = this.filteredUsers.sort(
    function(index1,index2){
      let user1 = this.users[index1]
      let user2 = this.users[index2]
      console.log(user1)
      switch(sortContents){
        case "first":
          return user1.name.first.toUpperCase() < user2.name.first.toUpperCase() ? -1:1
        case "last":
          return user1.name.last.toUpperCase() < user2.name.last.toUpperCase() ? -1:1
        case "state":
          return user1.location.state.toUpperCase() < user2.location.state.toUpperCase() ? -1:1
        case "country":
          return user1.location.country.toUpperCase() < user2.location.country.toUpperCase() ? -1:1
      }
    }.bind(this)
  )
}

export default {
  name: 'App',
  components: {
    contactItem
  },
  methods: {
    filterList,
    sortList
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
  background-image: url('assets/1x/outline_search_black_24dp.png');
  background-position: 10px 12px;
  background-repeat: no-repeat;
  background-color: white;
  width: 100%;
  font-size: 16px;
  padding: 12px 20px 12px 40px;
  border: 1px solid #ddd;
  margin-bottom: 12px;
}
#sort {
  background-color: white;
  width: 100%;
  font-size: 16px;
  padding: 12px 20px 12px 40px;
  border: 1px solid #ddd;
  margin-bottom: 12px;
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
