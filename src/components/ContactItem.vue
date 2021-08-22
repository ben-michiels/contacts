<template>
  <div class="contact-card">
    <md-list-item>
        <md-avatar>
          <img v-bind:src="user.picture.thumbnail" alt="People">
        </md-avatar>

        <div class="md-list-item-text">
          <span class="contact-name">{{`${user.name.first} ${user.name.last}`}}</span>
          <span class="contact-details">{{`${user.location.state} / ${user.location.country}`}}</span>
        </div>

        <md-icon>chevron_right</md-icon>
    </md-list-item>
    <md-divider class="md-inset"></md-divider>
  </div>
</template>

<script>
export default {
  name: 'ContactItem',
  props: 
  ['user']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.contact-name {
  color: #000000 !important;
  font-size: 1em !important;
  font-weight: bold;
}
.contact-details {
  color: #ff4081 !important;
  font-size: 0.90em !important;
}
.contact-card {
  background-color: white;
}
.md-list-item-text * {
  padding: 0.2em;
}
.contact-card {
  padding-top: 8px;
  margin-right: 8px;
}
.md-divider {
  margin-right: 20px;
}
</style>