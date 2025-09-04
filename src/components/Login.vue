<template>
  <!-- Main login form container -->
  <div class="login-container" data-testid="login-form">
    <h2 data-testid="login-title">QA Tester Login Test App</h2>

    <!-- Login form with validation -->
    <form @submit.prevent="handleLogin" data-testid="login-form-element">
      <!-- Username/Email input field -->
      <div class="form-group">
        <input
          type="email"
          id="username"
          data-testid="username-input"
          v-model="username"
          placeholder="Username (email)"
          :disabled="isLoading"
        />
        <!-- Username validation error message -->
        <p class="error" v-if="usernameError" data-testid="username-error">
          {{ usernameError }}
        </p>
      </div>

      <!-- Password input field -->
      <div class="form-group">
        <input
          type="password"
          id="password"
          data-testid="password-input"
          v-model="password"
          placeholder="Password"
          :disabled="isLoading"
        />
        <!-- Password validation error message -->
        <p class="error" v-if="passwordError" data-testid="password-error">
          {{ passwordError }}
        </p>
      </div>

      <!-- Login submit button -->
      <button
        type="submit"
        data-testid="login-button"
        :disabled="isLoading || hasValidationErrors"
        :class="{ loading: isLoading }"
      >
        {{ isLoading ? "Logging in..." : "Login" }}
      </button>
    </form>

    <!-- General login error message -->
    <p class="error" v-if="message" data-testid="login-error-message">
      {{ message }}
    </p>

    <!-- Test credentials display for QA testing -->
    <div class="test-credentials" data-testid="test-credentials">
      <p><strong>Test Credentials:</strong></p>
      <p>Email: test@example.com</p>
      <p>Password: Password123!</p>
    </div>
  </div>
</template>

<script>
/**
 * Login Component
 * Handles user authentication with email and password validation
 * Emits login-success event when authentication is successful
 */
export default {
  data() {
    return {
      // Form input fields
      username: "",
      password: "",

      // Error and status messages
      message: "",
      usernameError: "",
      passwordError: "",

      // Loading state for form submission
      isLoading: false,
    };
  },
  computed: {
    /**
     * Check if there are any validation errors
     * Used to disable the login button when form has errors
     */
    hasValidationErrors() {
      return this.usernameError !== "" || this.passwordError !== "";
    },
  },
  watch: {
    /**
     * Watch username field for changes and validate in real-time
     */
    username() {
      this.validateUsername();
      this.clearLoginError();
    },
    /**
     * Watch password field for changes and validate in real-time
     */
    password() {
      this.validatePassword();
      this.clearLoginError();
    },
  },
  methods: {
    /**
     * Validate email format and requirements
     */
    validateUsername() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      const trimmedEmail = this.username.trim();

      if (!trimmedEmail) {
        this.usernameError = "Email is required.";
      } else if (trimmedEmail.length > 254) {
        this.usernameError = "Email is too long.";
      } else if (!emailRegex.test(trimmedEmail)) {
        this.usernameError = "Please enter a valid email address.";
      } else {
        this.usernameError = "";
      }
    },

    /**
     * Validate password complexity requirements
     */
    validatePassword() {
      const password = this.password;

      if (!password) {
        this.passwordError = "Password is required.";
      } else if (password.length < 8) {
        this.passwordError = "Password must be at least 8 characters long.";
      } else if (password.length > 128) {
        this.passwordError = "Password is too long.";
      } else if (!/[A-Z]/.test(password)) {
        this.passwordError =
          "Password must contain at least one uppercase letter.";
      } else if (!/[a-z]/.test(password)) {
        this.passwordError =
          "Password must contain at least one lowercase letter.";
      } else if (!/[0-9]/.test(password)) {
        this.passwordError = "Password must contain at least one number.";
      } else if (!/[!@#$%^&*(),.?":{}|<>]/.test(password)) {
        this.passwordError =
          "Password must contain at least one special character.";
      } else {
        this.passwordError = "";
      }
    },

    /**
     * Clear the general login error message when user starts typing
     */
    clearLoginError() {
      if (this.message) {
        this.message = "";
      }
    },

    /**
     * Handle form submission and authentication
     * Simulates API call with loading state
     */
    async handleLogin() {
      // Clear any existing error message
      this.message = "";

      // Validate both fields before submission
      this.validateUsername();
      this.validatePassword();

      // Don't submit if there are validation errors
      if (this.usernameError || this.passwordError) {
        return;
      }

      // Show loading state
      this.isLoading = true;

      // Simulate API call delay for realistic testing experience
      await new Promise((resolve) => setTimeout(resolve, 1500));

      // Check credentials (case-insensitive email)
      const trimmedUsername = this.username.trim().toLowerCase();

      if (
        trimmedUsername === "test@example.com" &&
        this.password === "Password123!"
      ) {
        // Emit success event to parent component
        this.$emit("login-success", this.username.trim());
      } else {
        // Show error message for invalid credentials
        this.message = "Invalid email or password. Please try again.";
      }

      // Hide loading state
      this.isLoading = false;
    },
  },
};
</script>

<style scoped>
/* Metro Vancouver inspired login container */
.login-container {
  width: 500px; /* Fixed width - no max-width */
  padding: 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0, 63, 127, 0.15);
  border: 1px solid #e1ecf7;
  min-height: 500px; /* Fixed height to prevent resizing */
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

.login-container h2 {
  text-align: center;
  color: #003f7f;
  margin-bottom: 1.5rem;
  font-weight: 600;
}

/* Metro Vancouver blue form styling */
.form-group {
  margin-bottom: 1rem;
  height: 80px; /* Fixed height to prevent jumping */
}

input {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #cce0f3;
  border-radius: 4px;
  box-sizing: border-box;
  font-size: 1rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

input:focus {
  outline: none;
  border-color: #0066cc;
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.1);
}

input:disabled {
  background-color: #f8fafe;
  cursor: not-allowed;
}

/* Metro Vancouver blue button styling */
button {
  width: 100%;
  padding: 0.75rem;
  background-color: #0066cc;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: 600;
  margin-top: 1rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

button:hover:not(:disabled) {
  background-color: #0052a3;
}

button:disabled {
  background-color: #b3d1f0;
  cursor: not-allowed;
}

button.loading {
  background-color: #6699d6;
}

/* Error styling with Metro Vancouver accent */
.error {
  color: #c73e39;
  margin-top: 0.25rem;
  font-size: 0.875rem;
  height: 20px; /* Fixed height for error messages */
}

/* Metro Vancouver styled test credentials box */
.test-credentials {
  margin-top: 1.5rem;
  padding: 1rem;
  background-color: #f0f7ff;
  border: 1px solid #b3d1f0;
  border-radius: 4px;
  font-size: 0.875rem;
  color: #004080;
}

.test-credentials p {
  margin: 0.5rem 0;
}

.test-credentials p:first-child {
  font-weight: 600;
  color: #003f7f;
}

/* Responsive design */
@media (max-width: 768px) {
  .login-container {
    width: 90%;
    max-width: 400px;
    padding: 1.5rem;
    margin: 0 auto;
  }
}

@media (max-width: 480px) {
  .login-container {
    width: 95%;
    padding: 1rem;
  }

  .login-container h2 {
    font-size: 1.5rem;
  }

  input {
    font-size: 16px; /* Prevents zoom on iOS */
  }
}
</style>
