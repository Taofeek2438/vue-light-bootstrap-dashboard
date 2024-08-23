<template>
  <div class="wrapper">
    <login-navbar class="nav-bar"></login-navbar>
    <div class="main-panel">
      <div class="login-container">
        <div class="login-box">
          <img src="img/ftlogo.svg" alt="FirstTrustMortgage Bank" class="logo" />
          <h1>Login to ADMIN SPHERE</h1>
          <form @submit.prevent="login">
            <div class="form-group">
              <label for="email">Email:</label>
              <input type="email" v-model="email" required />
            </div>
            <div class="form-group">
              <label for="password">Password:</label>
              <input type="password" v-model="password" required />
            </div>
            <button type="submit" class="login-button" :disabled="loading">
              <span v-if="loading" class="loader"></span>
              <span v-else>Login</span>
            </button>
          </form>
          <div v-if="error" class="error">{{ error }}</div>
          <div v-if="success" class="success">{{ success }}</div>
        </div>
      </div>
    </div>
    <login-footer class="footer"></login-footer>
  </div>
</template>

<script>
import LoginFooter from '../layout/LoginFooter.vue';
import LoginNavbar from '../layout/LoginNavbar.vue';
import axios from 'axios';

export default {
  components: {
    LoginNavbar,
    LoginFooter,
  },
  data() {
    return {
      email: '',
      password: '',
      error: '',
      success: '',
      loading: false,
    };
  },
  methods: {
    async login() {
      this.loading = true;
      this.error = '';
      this.success = '';
      try {
        const response = await axios.post('http://10.100.2.31:7000/auth/loginLdap', {
          username: this.email,
          password: this.password,
        });

        const responseData = response.data;
        Object.keys(responseData).forEach((key) => {
          const value = responseData[key];
          localStorage.setItem(key, JSON.stringify(value));
        });

        if (this.password === 'ServiceDesk@123') {
          this.success = 'Kindly check your email to reset your password';
          setTimeout(() => {
            if (JSON.parse(localStorage.getItem('role') || '') === 'admin') {
              this.$router.push('/admin-dashboard');
            } else {
              this.$router.push('/dashboard');
            }
          }, 5000);
        } else {
          this.success = 'Login successful! Redirecting...';
          setTimeout(() => {
            if (JSON.parse(localStorage.getItem('role') || '') === 'admin') {
              this.$router.push('/admin');
            } else {
              this.$router.push('/dashboard');
            }
          }, 1000);
        }
      } catch (err) {
        this.error = 'Invalid email or password';
      } finally {
        this.loading = false;
      }
    },
  },
  clearFields() {
    this.email = '';
    this.password = '';
  }
};
</script>



<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-image: url('../assets/img/admin-background.jpg');
  background-attachment: fixed;
  background-size: cover;
  background-position: center;
}

.main-panel {
    flex: 1;
    display: flex;
    width: 100%;
    justify-content: center; /* Center login-box horizontally */
    align-items: center; /* Center login-box vertically within its flex container */
}

.footer {
  background-color: #fff; /* Background color */
  color: #004080; /* Text color */
  padding: 1rem; /* Padding around content */
  text-align: center; /* Center-align text */
  width: 100%; /* Full width of the viewport */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
  font-family: century-gothic, sans-serif;
  font-weight: 400;
}

.login-container {
  width: 100%;
  display: flex;
  justify-content: center; /* Center login-box horizontally */
  align-items: center; /* Center login-box vertically within its flex container */
}

.login-box {
  background-color: #fff;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  width: 20rem;
  text-align: center;
}

.logo {
  max-width: 100px;
  margin-bottom: 1rem;
}

h1 {
  color: #004080;
  margin-bottom: 1rem;
  font-size: larger;
}

.form-group {
  margin-bottom: 1rem;
  font-family: century-gothic, sans-serif;
  font-weight: 400;
  font-style: normal;
}

label {
  display: block;
  color: #004080;
  margin-bottom: 0.5rem;
  text-align: left;
}

input[type='email'],
input[type='password'] {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.forgot-password {
  text-align: left;
  margin-bottom: 1rem;
}

.forgot-password a {
  color: #004080;
  text-decoration: underline;
}

.login-button {
  background-color: #ffd700;
  color: #004080;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  width: 100%;
}

.login-button:hover {
  background-color: #ffc300;
}

.login-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.loader {
  border: 4px solid #f3f3f3; /* Light grey */
  border-top: 4px solid #004080; /* Blue */
  border-radius: 50%;
  width: 20px;
  height: 20px;
  animation: spin 2s linear infinite;
  display: inline-block;
  vertical-align: middle;
  margin-right: 10px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.error {
  color: red;
  margin-top: 1rem;
}

.success {
  color: green;
  margin-top: 1rem;
}
</style>
