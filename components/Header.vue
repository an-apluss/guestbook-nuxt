<template>
    <nav class="navbar navbar-expand-lg pt-3 pb-3">
      <span class="app-logo">GUESTBOOK</span>
      <button class="navbar-toggler border border-info" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse ml-5" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item active">
            <nuxt-link class="nav-link" to="/">Home <span class="sr-only">(current)</span></nuxt-link>
          </li>
          <li class="nav-item">
            <nuxt-link class="nav-link" to="/signatures/create">Create</nuxt-link>
          </li>
          <li class="nav-item">
            <nuxt-link class="nav-link" to="/signatures">Signatures</nuxt-link>
          </li>
        </ul>
      </div>
    </nav>
 </template>

<script>
export default {
  
}
</script>

<style scoped>
  nav {
    border-bottom: 1px solid #ffffff;
  }

  a {
    color: #ffffff;
    font-weight: bold;
    border-top: 1px solid transparent;
    border-bottom: 1px solid transparent;
    letter-spacing: 0.125em;
  }

  a:hover,
  a:active {
    color: #ffffff;
    border-top: 1px solid orangered;
    border-bottom: 1px solid orangered;
  }

  .app-logo {
    font-weight: bold;
    font-size: 1.5em;
    letter-spacing: 0.2em;
    color: orangered;
  }
</style>