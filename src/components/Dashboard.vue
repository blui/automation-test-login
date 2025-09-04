<template>
  <!-- Dashboard main container -->
  <div class="dashboard-container" data-testid="dashboard">
    <!-- Welcome section with user info -->
    <div class="welcome-section">
      <h2 data-testid="welcome-message">Welcome, {{ username }}</h2>
      <p data-testid="login-time">Logged in at: {{ loginTime }}</p>
    </div>

    <!-- Logout button -->
    <button
      @click="handleLogout"
      data-testid="logout-button"
      class="logout-btn"
    >
      Logout
    </button>
  </div>
</template>

<script>
/**
 * Dashboard Component
 * Displays welcome message and logout functionality after successful login
 * Shows the login timestamp for reference
 */
export default {
  // Receive username from parent component
  props: ["username"],

  data() {
    return {
      // Store login timestamp
      loginTime: "",
    };
  },

  /**
   * Set login timestamp when component is created
   */
  mounted() {
    this.loginTime = new Date().toLocaleString();
  },

  methods: {
    /**
     * Handle logout button click
     * Emits logout event to parent component
     */
    handleLogout() {
      this.$emit("logout");
    },
  },
};
</script>

<style scoped>
/* Main dashboard layout - centered content */
.dashboard-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  text-align: center;
  font-family: Arial, sans-serif;
  padding: 20px;
}

/* Welcome message section */
.welcome-section {
  margin-bottom: 30px;
}

.welcome-section h2 {
  color: #333;
  margin-bottom: 10px;
  font-size: 2em;
}

.welcome-section p {
  color: #666;
  font-size: 14px;
}

/* Logout button styling */
.logout-btn {
  padding: 12px 24px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  transition: background-color 0.3s;
}

.logout-btn:hover {
  background-color: #c82333;
}
</style>
