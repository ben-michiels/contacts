<template>
  <div id="app" class="page-container"> 
    
    <md-app md-waterfall md-mode="fixed">
      <md-app-toolbar md-theme="default-dark">
        <div class="md-toolbar-row md-layout md-alignment-bottom-left">
          <md-icon class="md-layout-item md-size-10 toolbar-icon">people</md-icon>
          <span class="md-title md-layout-item">My Contacts</span>
        </div>
        
        <div id="list-controls">
          <div id="searching" class="md-toolbar-row md-layout md-alignment-center-center">
            <p class="md-layout-item md-size-10 listing-label">Search</p>
            <md-field md-inline class="md-layout-item md-size-75 listing-bar">
              <input type="text" id="search" v-on:keyup="filterList()">
            </md-field>
          </div>
          <div id="sorting" class="md-toolbar-row md-layout md-alignment-center-center">
            <p class="md-layout-item md-size-10 listing-label">Sort</p>
            <form name="form1" id="form1" class="md-layout-item md-size-75 listing-bar">
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
              <div v-for="user of filteredUsers" v-bind:key="users[user].login.uuid">
                <ContactItem v-bind:user="users[user]" v-on:click.native="$emit('show-contact',users[user])"></ContactItem>
              </div>
            </md-list>                       
          </div>
        </md-app-content>
    </md-app>
  </div>
</template>

<script>
import ContactItem from './ContactItem.vue'

//Generates a list of indeces refering to the relevant users in the "users" array
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

//Sorts the current filtered list of indeces based on the selected option
function sortList() {
  let sortContents = document.getElementById("sort").value
  this.filteredUsers = this.filteredUsers.sort(
    function(index1,index2){
      let user1 = this.users[index1]
      let user2 = this.users[index2]
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
  name: 'ContactList',
  components: {
    ContactItem,
  },
  methods: {
    filterList,
    sortList
  },
  data: () => ({
    sort: "first name",
    filteredUsers: [],
  }),
  props: ["users"],
  //filterList is assigned to be run when users updates *and* when ContactList is mounted. On page loading, users only update after
  //mounting (on the API call). However, on returning from ContactDetails, users aren't updated so we filter on mounting.
  watch: {
    users:filterList
  },
  mounted(){
    filterList.bind(this)()
  }
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
.md-app-toolbar {
  gap: 1em;
  min-height: 5em;
  background-color: #212121;
  padding:0;
}
.md-toolbar-row {
  align-content: flex-end;
}
.toolbar-icon {
  margin: 0em;
  margin-left: 1em;
  color:white;
}
.md-title {
  color:white;
  font-size:1.4em;
}
#list-controls {
  padding-top: 0.2m;
  padding-left: 1em;
  background-color: #f4f4f4;
  min-width: 100%;
}
#searching,#sorting {
  margin-left:0;
  max-width: 100%;
  gap: 10%;
}
.listing-label {
  font-size: 1.2em;
}
.listing-bar {
  padding:0.5em;
}
#search {
  background-image: url('../assets/1x/outline_search_black_24dp.png');
  background-position: 10px 12px;
  background-repeat: no-repeat;
  background-color: white;
  width: 100%;
  font-size: 16px;
  padding: 16px 20px 12px 40px;
  border: 1px solid #ddd;
  margin-bottom: 2px;
}
#sort-bar {
  background-color: white;
}
#sort {
  background-color: white;
  width: 100%;
  font-size: 16px;
  padding: 16px 20px 12px 10px;
  border: 1px solid #ddd;
  margin-bottom: 2px;
}
#scrolling-list {
  background-color:white;
  min-height:100%;
}
.md-list {
  padding:0;
}
</style>