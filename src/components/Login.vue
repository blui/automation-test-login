<template>
  <!-- Main login form container -->
  <div class="login-container" data-testid="login-form">
    <h2 data-testid="login-title">Login</h2>

    <!-- Login form with validation -->
    <form @submit.prevent="handleLogin" data-testid="login-form-element">
      <!-- Username/Email input field -->
      <div class="form-group">
        <label for="username" data-testid="username-label"
          >Username (Email)</label
        >
        <input
          type="email"
          id="username"
          data-testid="username-input"
          v-model="username"
          placeholder="Enter your email address"
          :disabled="isLoading"
        />
        <!-- Username validation error message -->
        <p class="error" v-if="usernameError" data-testid="username-error">
          {{ usernameError }}
        </p>
      </div>

      <!-- Password input field -->
      <div class="form-group">
        <label for="password" data-testid="password-label">Password</label>
        <input
          type="password"
          id="password"
          data-testid="password-input"
          v-model="password"
          placeholder="Enter your password"
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
/* Main login form container */
.login-container {
  width: 400px;
  margin: 50px auto;
  padding: 30px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  font-family: Arial, sans-serif;
}

/* Form field grouping */
.form-group {
  margin-bottom: 20px;
}

/* Form labels */
label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
  color: #333;
}

/* Input field styling */
input {
  width: 100%;
  padding: 12px;
  border: 2px solid #ddd;
  border-radius: 4px;
  box-sizing: border-box;
  font-size: 14px;
  transition: border-color 0.3s;
}

/* Input focus state */
input:focus {
  outline: none;
  border-color: #42b983;
}

/* Disabled input state */
input:disabled {
  background-color: #f5f5f5;
  cursor: not-allowed;
}

/* Submit button styling */
button {
  width: 100%;
  padding: 12px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  transition: background-color 0.3s;
}

/* Button hover state (only when enabled) */
button:hover:not(:disabled) {
  background-color: #369f77;
}

/* Disabled button state */
button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

/* Loading button state */
button.loading {
  background-color: #999;
}

/* Error message styling */
.error {
  color: #dc3545;
  margin-top: 8px;
  font-size: 14px;
  line-height: 1.4;
}

/* Test credentials display box */
.test-credentials {
  margin-top: 30px;
  padding: 15px;
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  font-size: 12px;
  color: #6c757d;
}

.test-credentials p {
  margin: 5px 0;
}
</style>
