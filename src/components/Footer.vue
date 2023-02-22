<template>
  <img src="../assets/taxi.png" alt="taxi pattern">
    <footer>
        <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
        <img src="../assets/taxi.png" alt="taxi pattern">
      <router-link to="/">
        Home
      </router-link>
  
      <ul>
          <li>
            <router-link to="/">Task Manager</router-link>
          </li>
          <li>
            <router-link to="/clock">Clock</router-link>
          </li>
          <li>
            <router-link to="/account">Profile</router-link>
          </li>
      </ul>
  
      <div>
        <ul>
          <li class="log-out-welcome">
            <p>Hi, user</p>
          </li>
          <li>
            <button @click="signOut" class="button">Log out</button>
          </li>
        </ul>
      </div>
    </footer>
  </template>
  
<style>
footer {
  background-color: #F4AD28;
  display: flex;
	flex-direction: row;
	flex-wrap: nowrap;
	justify-content: space-around;
	align-items: center;
	align-content: stretch;
  margin-left: 15%;
  margin-right: 15%;
}
</style>