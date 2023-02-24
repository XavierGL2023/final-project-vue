  <template>
    <div class="one-element">
      <div class="centered">
        <h3>NewYork ToDo App</h3>
      <div class="container-signin">
        <h2 class="header-title">Sign In</h2>
        <form @submit.prevent="signIn" class="form-sign-in">
        <div class="form">
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
      </div>
      <button class="button-signin" type="submit"></button>
    </form>
  </div>
  <p class="signup">
          Have an account?
          <PersonalRouter
            :route="route"
            :buttonText="buttonText"
            class="sign-up-link"
          />
        </p>
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

const route = "/auth/signup";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
const signIn = async () => {
    try {
      // calls the user store and send the users info to backend to logIn
      await useUserStore().signIn(email.value, password.value);
      // redirects user to the homeView
      redirect.push({ path: "/" });
    } catch (error) {
      // displays error message
      errorMsg.value = error.message;
      // hides error message
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }

  errorMsg.value = "error";
};
</script>

<style>

body {
  background-color: #FFDD0B;
}

.button-signin {
    background-image: url(../assets/login.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
}

.container-signin{
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
    box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
    border: 3px solid #000000;
    margin-top: 3%;
}

@media (max-width: 768px) {
  .container-signin {
    width: 100%;
  }
}


.signup{
  text-align: center;
  margin-top: 3%;
} 

.one-element {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Opcional: establece la altura del contenedor para que ocupe todo el viewport */
}

.centered {
  /* Opcional: ajusta el tamaño y el margen del elemento que deseas centrar */
  width: 50%;
  height: 50%;
  margin: auto;
}

.icons3 {
  display: flex;
    flex-direction: row;
    align-items: flex-end;
    justify-content: space-between;
    margin-top: 5%;
}

.form-sign-in {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
