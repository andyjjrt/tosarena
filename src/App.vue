<template>
  <div id="app">
    <nav class="navbar navbar-light bg-light fixed-top">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">
          <img src="./assets/icon.png" alt="" width="40" height="40">
          &nbsp;競技場計算機
        </a>
      </div>
    </nav>
    <br/><br><br>
    <Page />
  </div>
</template>

<script>
import Page from './components/Page.vue'

export default {
  name: 'App',
  components: {
    Page
  }
}
</script>
