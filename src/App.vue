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
  <!-- Main application container -->
  <div class="app" data-testid="app">
    <!-- Application header -->
    <header class="app-header" data-testid="app-header">
      <h1>Test Login Application</h1>
    </header>

    <!-- Main content area - shows Login or Dashboard based on authentication state -->
    <main class="main-content" data-testid="main-content">
      <!-- Login view - shown when user is not authenticated -->
      <Login
        v-if="!loggedIn"
        @login-success="handleLoginSuccess"
        data-testid="login-view"
      />

      <!-- Dashboard view - shown when user is authenticated -->
      <Dashboard
        v-else
        :username="username"
        @logout="handleLogout"
        data-testid="dashboard-view"
      />
    </main>

    <!-- Application footer -->
    <footer class="app-footer" data-testid="app-footer">
      <p>QA Interview Test Application</p>
    </footer>
  </div>
</template>

<style scoped>
/* Main application layout */
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  font-family: Arial, sans-serif;
}

/* Application header styling */
.app-header {
  background-color: #2c3e50;
  color: white;
  padding: 20px;
  text-align: center;
}

.app-header h1 {
  margin: 0;
  font-size: 1.5em;
}

/* Main content area - takes remaining space and centers content */
.main-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f8f9fa;
}

/* Application footer styling */
.app-footer {
  background-color: #6c757d;
  color: white;
  padding: 10px;
  text-align: center;
  font-size: 12px;
}

.app-footer p {
  margin: 0;
}
</style>
