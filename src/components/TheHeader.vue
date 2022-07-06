<template>
  <nav>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">EXCHANGE</button>

    <div>
      <select name="languages">
        <option value="eng">eng</option>
        <option value="ru">ru</option>
        <option value="uz">uz</option>
      </select>
      <select name="companies">
        <option value="default-company">Company</option>
      </select>
      <button class="login">Login/Sign up</button>
    </div>
  </nav>
</template>
<script setup>
</script>
<style lang="css" scoped>
  nav {
    padding: 0 15px;
    border-top: 2px solid #999;
    border-bottom: 1px solid #999;
    display: flex;
    justify-content: space-between;
  }

  div {
    width: 55%;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .login {
    font-size: 18px;
    border: none;
    background-color: transparent;
  }

  select {
    border: none;
    font-size: 18px;
  }
</style>