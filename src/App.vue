<script setup>
</script>

<template>
  <div>
    <div id="app">
      <div id="container">
        <h1>Building visit book</h1>
        <div id="combo">
          <b-img thumbnail class="d-none d-lg-block" id="image" v-bind:src="require('./images/lobby.jpg')"
            alt="Image 1"></b-img>
          <router-view />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
#app {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #F2F2F2;
}

#container {
  display: flex;
  flex-direction: column;
}

#combo {
  display: flex;
  flex-direction: row;
}

#image {
  width: 30%;
  height: 80vh;
  object-fit: cover;
}
</style>
