<template>
  <div id="app">
    <div class="dashboard">
      <div class="card">
        <TransactionTicker />
      </div>
      <div class="card">
        <BloodlineDatabase />
      </div>
      <div class="card">
        <UserRegistration />
      </div>
      <div class="card">
        <LitterRegistration />
      </div>
      <div class="card">
        <SingleDogRegistration />
      </div>
    </div>
  </div>
</template>

<script>
import TransactionTicker from "./components/TransactionTicker.vue"
import BloodlineDatabase from "./components/BloodlineDatabase.vue"
import UserRegistration from './components/UserRegistration.vue'
import LitterRegistration from './components/LitterRegistration.vue'
import SingleDogRegistration from './components/SingleDogRegistration.vue'

export default {
  name: 'App',
  components: {
    TransactionTicker,
    BloodlineDatabase,
    UserRegistration,
    LitterRegistration,
    SingleDogRegistration
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  grid-gap: 20px;
  justify-items: center;
}

.card {
  background-color: #f0f0f0;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
</style>
