<!-- COMPONENTE BOILERPLATE -->
 
  <template>
  <div class="container-signin">
      <h3 class="header-title">Log In to ToDo App</h3>
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
        <button class="button-signin" type="submit">Sign In</button>
      </div>
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
.container-signin{
  margin: auto;
    margin-top: 15%;
    width: 300px;
    background-color: #F4AD28;
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
    justify-content: space-evenly;
    align-items: center;
    align-content: stretch;
    padding: 2%;
    border-radius: 1em;
}

.button-signin {
  margin: auto;
}
</style>
