<template>
    <div class="page-container"> 
        <md-app md-waterfall md-mode="fixed">
            <md-app-toolbar md-theme="default-dark">
                <div class="md-toolbar-row md-layout md-alignment-bottom-left">
                    <md-icon class="md-layout-item md-size-10 toolbar-icon" v-on:click.native="$emit('show-list')">arrow_back</md-icon> 
                    <span class="md-title md-layout-item">Contact</span>
                </div>
            </md-app-toolbar>
            <md-app-content>
                <div class="large-image-container">
                    <img id="profile-picture" class="cropped-image" v-bind:src="this.user.picture.large" v-on:click="toggleImageSize" alt="Profile picture">
                    <h2 id=contact-name>{{user.name.first}} {{user.name.last}}</h2>
                </div>
                <main class="details">
                    <dl>
                        <dt>Age</dt>    <dd>{{user.dob.age}}</dd>
                        <dt>Email</dt>  <dd>{{user.email}}</dd>
                        <dt>Mobile</dt> <dd>{{user.cell}}</dd>
                        <dt>Phone</dt>  <dd>{{user.phone}}</dd>
                    </dl>
                    <address>
                        <dl>
                            <dt>Address</dt>    
                            <dd>{{user.location.street.number}} {{user.location.street.name}}<br>
                            {{user.location.city}}<br>
                            {{user.location.state}}<br>
                            {{user.location.postcode}} {{user.location.country}}</dd>
                        </dl>
                    </address>
                </main>
                <md-bottom-bar class="md-accent" md-sync-route>
                    <md-bottom-bar-item md-label="MOBILE" md-icon="smartphone" v-bind:href="'tel:+'+user.cell" class="contact-link"></md-bottom-bar-item>
                    <md-bottom-bar-item md-label="PHONE" md-icon="phone" v-bind:href="'tel:+'+user.phone" class="contact-link"></md-bottom-bar-item>
                    <md-bottom-bar-item md-label="EMAIL" md-icon="email" v-bind:href="'mailto:'+user.email" class="contact-link"></md-bottom-bar-item>
                </md-bottom-bar>
            </md-app-content>
        </md-app>
    </div>
</template>

<script>
export default {
  name: 'ContactDetails',
  props: 
  ['user'],
  methods: {
      toggleImageSize: function(){
        document.getElementById("profile-picture").classList.toggle("cropped-image")
      }
  }
}
</script>

<style scoped>
.md-app {
  height: 100vh;
}
.md-toolbar-row {
    padding-bottom: 0.5em;
}
.toolbar-icon {
    font-size:30px !important;
    margin-left: 24px;
    padding-bottom: 0.1em;
}
.large-image-container {
    position: relative;
    z-index:0;
    height:auto
}
#profile-picture {
    position:relative;
    width: 100vw;
    z-index: 1;
}
.cropped-image {
    max-height: 32vh;
    overflow: hidden;
    object-fit:cover;
}
#contact-name {
    color: white;
    font-size: 1.8em;
    position: absolute;
    padding-bottom: 1em;
    padding-left: 1em;
    z-index: 2;
    bottom:0;
    margin:0;
    font-weight:500;
    text-shadow: 1px 1px 1px #212121;
}
dl {
    display: flex;
    flex-flow: row wrap;
  }
dt {
    flex-basis: 20%;
    padding: 2px 4px;
    text-align: left;
    color: #ff4081;
    font-size: 0.90em;
}
dd {
    flex-basis: 70%;
    flex-grow: 1;
    margin: 0;
    padding: 2px 4px;
    color: #212121;
    font-size: 1em;
    font-weight: bold;
}
address {
    font-style: normal;
}
.contact-details {
  background-color: white;
}
main {
    padding: 1em;
    margin-bottom: 56px;
}
.md-bottom-bar {
    bottom:0;
    position:fixed;
}
</style>