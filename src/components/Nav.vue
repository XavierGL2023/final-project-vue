<template>
  <nav>
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <router-link to="/">
      <a href="/"><img class="logo-home" src="../assets/esb.svg" alt="logo-home"></a>
    </router-link>

    <ul>
        <li>
          <router-link to="/"><h3>Task Manager</h3></router-link>
        </li>
        </ul>
        <ul>
        <li>
          <a href="#clock_ny"><h3>Clock</h3></a>
        </li>
      </ul>
      <ul>
        <li>
          <router-link to="/account"><h3>Profile</h3></router-link>
        </li>
    </ul>

    <div>
      <ul>
        <li class="log-out-welcome">
          <p>Hi, {{ userEmail }}</p>
        </li>
        <li>
          <button @click="signOut" class="logout_button"></button>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from 'vue';

//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";

// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;

// constant that calls user email from the useUSerStore
const userEmail = getUser.email;

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  try{
    // call the user store and send the users info to backend to signOut
    await useUserStore().signOut();
    // then redirect user to the homeView
    redirect.push({ path: "/auth/login" });

  } catch (error) {}
};

</script>

<style>

@media screen and (min-width: 768px) {
  .menu-button {
    display: none;
  }
  .menu-items {
    display: flex !important;
  }
}
@media screen and (max-width: 767px) {
  .menu-items {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    z-index: 1;
    background-color: #F4AD28;
    flex-direction: column;
    align-items: center;
    padding: 0;
  }
  .menu-items.show-menu {
    display: flex;
  }
  .menu-items li {
    width: 100%;
    text-align: center;
    margin-bottom: 1rem;
  }
}
.navbar-img {
  width: 90px;
}

.logo-home{
width: 100px;
}

.logout_button {
    background-image: url(../assets/logout.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
}

nav {
  background-color: #F4AD28;
  display: flex;
	flex-direction: row;
	flex-wrap: nowrap;
	justify-content: space-around;
	align-items: center;
	align-content: stretch;
  margin-left: 15%;
  margin-right: 15%;
  box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
}

@media (max-width: 768px) {
  nav {
    display: flex;
    flex-direction: row;
  margin-left: 0;
    margin-right: 0;
  }
}

nav ul {
  list-style: none;
  padding-inline-start: 0;
  display: flex;
  flex-direction: row;
  align-items: center;
	flex-wrap: nowrap;
	justify-content: space-around;
	align-content: stretch;
}

</style>
