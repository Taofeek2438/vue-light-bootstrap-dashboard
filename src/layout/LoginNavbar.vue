<template>
    <nav class="navbar navbar-expand-lg">
      <div class="container-fluid">
        <img class="logo" src="img/favicon.svg" alt="">
        <a class="navbar-brand" href="#">ADMIN SHPERE</a>
      </div>
    </nav>
  </template>
  <script>
    export default {
      computed: {
        routeName () {
          const {name} = this.$route
          return this.capitalizeFirstLetter(name)
        }
      },
      data () {
        return {
          activeNotifications: false
        }
      },
      methods: {
        capitalizeFirstLetter (string) {
          return string.charAt(0).toUpperCase() + string.slice(1)
        },
      }
    }
  
  </script>
  <style>
  .logo {
    width: 125px;
    padding-right: 10px;
    height: auto;
  }
  .navbar-brand {
    text-align: center;
  }
  </style>
  