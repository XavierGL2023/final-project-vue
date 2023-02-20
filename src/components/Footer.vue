<template>
    <footer>
        <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
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