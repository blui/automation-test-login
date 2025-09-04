<script setup>
/**
 * Main App Component
 * Manages application state and navigation between Login and Dashboard views
 * Uses Vue 3 Composition API with script setup syntax
 */
import { ref } from "vue";
import Login from "./components/Login.vue";
import Dashboard from "./components/Dashboard.vue";

// Application state - tracks if user is logged in
const loggedIn = ref(false);

// Store the logged-in username
const username = ref("");

/**
 * Handle successful login from Login component
 * Switch to dashboard view and store username
 */
const handleLoginSuccess = (user) => {
  loggedIn.value = true;
  username.value = user;
};

/**
 * Handle logout from Dashboard component
 * Reset application state to login view
 */
const handleLogout = () => {
  loggedIn.value = false;
  username.value = "";
};
</script>

<template>
  <div id="app">
    <Login v-if="!loggedIn" @login-success="handleLoginSuccess" />
    <Dashboard v-else :username="username" @logout="handleLogout" />
  </div>
</template>
<style scoped>
#app {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  color: #003f7f;
  margin-top: 60px;
}
</style>
