<script setup>
import TheHeader from './components/TheHeader'
import TheCard from './components/TheCard'

</script>

<template>
  <div class="container">
    <the-header />
    <main>
      <the-card />
    </main>
  </div>
</template>

<style lang="css" scoped>
  .container {
    width: 600px;
    height: auto;
    margin: 70px auto 0 auto;
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  }

  main {
    width: 100%;
    height: 100%;
    padding-top: 70px;
    padding-bottom: 30px;
    background-image: url('./assets/background-img.jpg');
    background-repeat: no-repeat;
    background-size: cover;
  }
</style>
