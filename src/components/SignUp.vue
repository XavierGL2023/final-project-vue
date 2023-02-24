<template>
  <div class="one-element">
      <div class="centered">
        <h3>NewYork ToDo App</h3>
    <div class="container-signup">
      <div class="header">
        <div class="header-description">
          <h2 class="header-title">Sign Up</h2>
        </div>
      </div>
      <form @submit.prevent="signUp" class="form-sign-up">
        <div class="form">
        </div>
         <div class="form-input">
            <label class="input-field-label">E-mail</label>
            <input
            type="email"
            class="input-field"
            placeholder="example@gmail.com"
            id="email"
            v-model="email"
            required
          />
        </div>
        <div class="form-input">
          <label class="input-field-label">Password</label>
          <input
            type="password"
            class="input-field"
            placeholder="**********"
            id="password"
            v-model="password"
            required
          />
        </div>
        <div class="form-input">
          <label class="input-field-label">Confirm password</label>
          <input
            type="password"
            class="input-field"
            placeholder="**********"
            id="confirmPassword"
            v-model="confirmPassword"
            required
          />
        </div>
        <button class="button-signup" type="submit"></button>
    </form>
      </div>
      <p class="account">
          Have an account?
          <PersonalRouter
            :route="route"
            :buttonText="buttonText"
            class="sign-up-link"
          />
        </p>


    <div v-show="errorMsg">{{errorMsg}}</div>
    <div class="icons3">
    <img class="ny-icon1" src="../assets/liberty_icon.svg" alt="Liberty-icon">
    <img class="ny-icon1" src="../assets/taxi_icon.svg" alt="Taxi-icon">
    <img class="ny-icon1" src="../assets/esb_icon.svg" alt="Empire-State-Building-icon">
   </div>
   <div class="linea"></div>
    </div>
    </div>
    
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/login";
const buttonText = "Sign In";

// Input Fields
const email = ref("");
const password = ref("");
const confirmPassword = ref("");

// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
const signUp = async () => {
  if (password.value === confirmPassword.value) {
    try {
      // calls the user store and send the users info to backend to logIn
      await useUserStore().signUp(email.value, password.value);
      // redirects user to the homeView
      redirect.push({ path: "/auth/login" });
    } catch (error) {
      // displays error message
      errorMsg.value = error.message;
      // hides error message
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  errorMsg.value = "error";
};
</script>

<style>

body {
  background-color: #FFDD0B;
}

h3 {
  text-align: center;
}

.button-signup {
  background-image: url(../assets/login.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
}

.container-signup{
  margin: auto;
  width: 30%;
  height: 50%;
    background-color: #F4AD28;
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
    justify-content: space-evenly;
    align-items: center;
    align-content: stretch;
    padding: 2%;
    border-radius: 15px;
    border: 3px solid #000000;
    box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
    margin-top:3%;
}

@media (max-width: 768px) {
  .container-signup {
    width: 100%;
  }
}

.account{
  text-align: center;
  margin-top: 3%;
}

.form-sign-up {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
