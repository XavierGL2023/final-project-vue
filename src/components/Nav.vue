<template>
  <nav>
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <router-link to="/">
      <a href="/"><img class="logo-home" src="../assets/esb.svg" alt="logo-home"></a>
    </router-link>

    <ul class="lista-nav">
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
        <li>
          <button @click="signOut" class="logout_button"></button>
        </li>
      </ul>
    </div>
  </nav>

  <div class="burguer-menu">
        <div class="container-b">
          <router-link to="/">
          <a href="/"><img class="logo-home-b" src="../assets/esb.svg" alt="logo-home"></a>
          </router-link>

            <input class="checkbox" type="checkbox" name="" id="" />
            <div class="hamburger-lines">
              <span class="line line1"></span>
              <span class="line line2"></span>
              <span class="line line3"></span>
            </div>  
          <div class="menu-items">
            <li><router-link to="/"><h3>Task Manager</h3></router-link></li>
            <li><a href="#clock_ny"><h3>Clock</h3></a></li>
            <li><router-link to="/account"><h3>Profile</h3></router-link></li>
            <li><button @click="signOut" class="logout_button"></button></li>        
                 </div>
        </div>
      </div>

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
box-shadow: 3px 3px rgb(0 0 0 / 40%);
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
  height: 130px;
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





.container-b {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 62px;
  margin: auto;
 }

.burguer-menu {
  display: none;
  box-shadow: 3px 3px rgb(0 0 0 / 40%);
}

.menu-items {
  display: none;
}

.burguer-menu .container-b li {
  list-style: none;
}

.burguer-menu .container-b a {
  text-decoration: none;
  color: black;
  font-weight: 500;
  font-size: 1.2rem;
}

.menu-items a:hover{
   font-weight: bolder;
}

.container-b {
  position: relative;
  height: 100px;
}

 .checkbox {
  position: absolute;
  display: block;
  height: 32px;
  width: 32px;
  top: 20px;
  right: 20px;
  z-index: 5;
  opacity: 0;
  cursor: pointer;
}

.hamburger-lines {
  display: block;
  height: 26px;
  width: 32px;
  position: absolute;
  top: 17px;
  right: 20px;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.hamburger-lines .line {
  display: block;
  height: 4px;
  width: 100%;
  border-radius: 10px;
  background: #0e2431;
}
 .hamburger-lines .line1 {
  transform-origin: 0% 0%;
  transition: transform 0.4s ease-in-out;
}

 .hamburger-lines .line2 {
  transition: transform 0.2s ease-in-out;
}

.hamburger-lines .line3 {
  transform-origin: 0% 100%;
  transition: transform 0.4s ease-in-out;
}

 .menu-items {
  padding: 20px;
  width: 100%;
  transform: translateY(-150%);
  display: flex;
  flex-direction: column;
  transition: transform 0.5s ease-in-out;
  text-align: center;
   
}

.menu-items li {
  margin-bottom: 1.2rem;
  font-size: 1.5rem;
  font-weight: 500;
}

.container-b input[type="checkbox"]:checked ~ .menu-items {
  transform: translateY(70px);
}

.container-b input[type="checkbox"]:checked ~ .hamburger-lines .line1 {
  transform: rotate(45deg);
}

.container-b input[type="checkbox"]:checked ~ .hamburger-lines .line2 {
  transform: scaleY(0);
}

.container-b input[type="checkbox"]:checked ~ .hamburger-lines .line3 {
  transform: rotate(-45deg);
}

.container-b input[type="checkbox"]:checked ~ .logo{
  display: none;
}




@media (max-width: 768px) {
  .burguer-menu {
    display: flex;
    background-color: #F4AD28;
  }

  .menu-items {
    display: flex;
  }

  .logo-home-b {
   width: 70px;
   height: 70px;
   box-shadow: 3px 3px rgb(0 0 0 / 40%);
  }

  nav {
    display: none;
  }
}

</style>
