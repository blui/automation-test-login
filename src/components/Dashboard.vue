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
/* Metro Vancouver inspired dashboard container */
.dashboard-container {
  width: 500px; /* Fixed width to match login container */
  padding: 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0, 63, 127, 0.15);
  border: 1px solid #e1ecf7;
  text-align: center;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

/* Metro Vancouver blue welcome section */
.welcome-section {
  margin-bottom: 2rem;
}

.welcome-section h2 {
  color: #003f7f;
  margin-bottom: 1rem;
  font-weight: 600;
}

.welcome-section p {
  color: #004080;
  margin: 0;
}

/* Metro Vancouver styled logout button */
.logout-btn {
  padding: 0.75rem 1.5rem;
  background-color: #c73e39;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: 600;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

.logout-btn:hover {
  background-color: #a32e29;
}
</style>
