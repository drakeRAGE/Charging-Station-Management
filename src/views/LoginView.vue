<template>
  <div class="login-container">
    <!-- Animated Background Elements -->
    <div class="bg-decoration">
      <div class="floating-orb orb-1"></div>
      <div class="floating-orb orb-2"></div>
      <div class="floating-orb orb-3"></div>
      <div class="grid-pattern"></div>
    </div>

    <!-- Particle System -->
    <div class="particles">
      <div class="particle" v-for="n in 20" :key="n" :style="getParticleStyle(n)"></div>
    </div>

    <div class="login-wrapper">

      <!-- Login Form Card -->
      <div class="login-card">
        <form @submit.prevent="handleLogin" class="login-form">
          <div class="form-header">
            <h2>Sign In</h2>
            <p>Enter your credentials to access your account</p>
          </div>

          <div class="form-content">
            <div class="form-group">
              <div class="input-wrapper">
                <div class="input-icon">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path>
                    <polyline points="22,6 12,13 2,6"></polyline>
                  </svg>
                </div>
                <input type="email" id="email" v-model="email" required placeholder="Enter your email"
                  :class="{ 'has-value': email }" />
                <label for="email">Email Address</label>
              </div>
            </div>

            <div class="form-group">
              <div class="input-wrapper">
                <div class="input-icon">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                    <circle cx="12" cy="16" r="1"></circle>
                    <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                  </svg>
                </div>
                <input :type="showPassword ? 'text' : 'password'" id="password" v-model="password" required
                  placeholder="" :class="{ 'has-value': password }" />
                <label for="password">Password</label>
                <button type="button" class="password-toggle" @click="showPassword = !showPassword">
                  <svg v-if="showPassword" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path
                      d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24">
                    </path>
                    <line x1="1" y1="1" x2="23" y2="23"></line>
                  </svg>
                  <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                    <circle cx="12" cy="12" r="3"></circle>
                  </svg>
                </button>
              </div>
            </div>


            <button type="submit" class="btn btn-primary" :disabled="isLoading">
              <div v-if="isLoading" class="loading-spinner">
                <div class="spinner"></div>
              </div>
              <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"></path>
                <polyline points="10,17 15,12 10,7"></polyline>
                <line x1="15" y1="12" x2="3" y2="12"></line>
              </svg>
              {{ isLoading ? 'Signing In...' : 'Sign In' }}
            </button>
          </div>
        </form>

        <div class="form-footer">
          <p class="register-link">
            Don't have an account?
            <router-link to="/register" class="link-primary">Create one now</router-link>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginView',
  data() {
    return {
      email: '',
      password: '',
      showPassword: false,
      rememberMe: false,
      isLoading: false
    }
  },
  methods: {
    async handleLogin() {
      this.isLoading = true;

      try {
        const apiUrl = import.meta.env.VITE_API_URL;
        const response = await fetch(`${apiUrl}/api/auth/login`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            email: this.email,
            password: this.password
          })
        });

        const data = await response.json();

        if (response.ok) {
          localStorage.setItem('token', data.token);
          this.showNotification('Login successful', 'success');
          this.$router.push('/profile');
        } else {
          this.showNotification(data.message || 'Login failed', 'error');
        }
      } catch (error) {
        console.error('Login error:', error);
        this.showNotification('An error occurred during login', 'error');
      } finally {
        this.isLoading = false;
      }
    },

    showNotification(message, type) {
      // Simple notification - you can replace with a proper notification system
      alert(message);
    },

    getParticleStyle(index) {
      return {
        left: `${Math.random() * 100}%`,
        animationDelay: `${Math.random() * 2}s`,
        animationDuration: `${3 + Math.random() * 2}s`
      };
    }
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.login-container {
  position: relative;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

/* Animated Background */
.bg-decoration {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: 1;
}

.floating-orb {
  position: absolute;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  animation: float 6s ease-in-out infinite;
}

.orb-1 {
  width: 300px;
  height: 300px;
  top: -10%;
  left: -10%;
  animation-delay: 0s;
}

.orb-2 {
  width: 200px;
  height: 200px;
  top: 60%;
  right: -10%;
  animation-delay: 2s;
}

.orb-3 {
  width: 150px;
  height: 150px;
  top: 20%;
  left: 70%;
  animation-delay: 4s;
}

.grid-pattern {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image:
    linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
  background-size: 60px 60px;
  animation: slideGrid 25s linear infinite;
}

/* Particle System */
.particles {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.particle {
  position: absolute;
  width: 4px;
  height: 4px;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 50%;
  animation: particleFloat 5s ease-in-out infinite;
}

@keyframes float {

  0%,
  100% {
    transform: translateY(0) rotate(0deg);
  }

  50% {
    transform: translateY(-30px) rotate(180deg);
  }
}

@keyframes slideGrid {
  0% {
    transform: translate(0, 0);
  }

  100% {
    transform: translate(60px, 60px);
  }
}

@keyframes particleFloat {

  0%,
  100% {
    transform: translateY(100vh) scale(0);
    opacity: 0;
  }

  10%,
  90% {
    opacity: 1;
  }

  50% {
    transform: translateY(-10px) scale(1);
  }
}

/* Main Content */
.login-wrapper {
  position: relative;
  z-index: 2;
  width: 100%;
  max-width: 450px;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

/* Brand Section */
.brand-section {
  text-align: center;
  color: white;
}

.logo-container {
  margin-bottom: 1.5rem;
}

.logo {
  display: inline-block;
  width: 80px;
  height: 80px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 20px;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.logo-inner {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-inner svg {
  width: 40px;
  height: 40px;
  color: white;
}

.brand-title {
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0 0 0.5rem 0;
  background: linear-gradient(135deg, #fff, #f0f0f0);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

.brand-subtitle {
  font-size: 1.125rem;
  color: rgba(255, 255, 255, 0.8);
  margin: 0;
  font-weight: 400;
}

/* Login Card */
.login-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  border-radius: 24px;
  padding: 0;
  box-shadow:
    0 25px 50px rgba(0, 0, 0, 0.15),
    0 0 0 1px rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
  overflow: hidden;
  transition: all 0.3s ease;
}

.login-card:hover {
  transform: translateY(-5px);
  box-shadow:
    0 30px 60px rgba(0, 0, 0, 0.2),
    0 0 0 1px rgba(255, 255, 255, 0.3);
}

/* Form Sections */
.form-header {
  padding: 2.5rem 2.5rem 1rem 2.5rem;
  text-align: center;
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.form-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #1f2937;
  margin: 0 0 0.5rem 0;
}

.form-header p {
  color: #6b7280;
  margin: 0;
  font-size: 0.975rem;
}

.form-content {
  padding: 2rem 2.5rem;
}

.form-footer {
  padding: 1.5rem 2.5rem 2.5rem 2.5rem;
  border-top: 1px solid rgba(0, 0, 0, 0.05);
  background: rgba(249, 250, 251, 0.8);
  text-align: center;
}

/* Form Groups */
.form-group {
  margin-bottom: 1.5rem;
}

.input-wrapper {
  position: relative;
}

.input-wrapper input {
  width: 100%;
  height: 56px;
  padding: 1rem 1rem 1rem 3.5rem;
  border: 2px solid #e5e7eb;
  border-radius: 12px;
  font-size: 1rem;
  background: rgba(255, 255, 255, 0.8);
  transition: all 0.3s ease;
  outline: none;
}

.input-wrapper input:focus {
  border-color: #667eea;
  background: white;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.input-wrapper input.has-value,
.input-wrapper input:focus {
  padding-top: 1.5rem;
  padding-bottom: 0.5rem;
}

.input-wrapper label {
  position: absolute;
  left: 3.5rem;
  top: 50%;
  transform: translateY(-50%);
  color: #6b7280;
  font-size: 1rem;
  font-weight: 500;
  transition: all 0.3s ease;
  pointer-events: none;
  background: white;
  padding: 0 0.5rem;
}

.input-wrapper input:focus+label,
.input-wrapper input.has-value+label {
  top: 0.75rem;
  font-size: 0.75rem;
  color: #667eea;
  font-weight: 600;
}

.input-icon {
  position: absolute;
  left: 1rem;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  color: #9ca3af;
  z-index: 2;
}

.input-icon svg {
  width: 100%;
  height: 100%;
}

.password-toggle {
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  width: 20px;
  height: 20px;
  color: #9ca3af;
  cursor: pointer;
  transition: color 0.3s ease;
}

.password-toggle:hover {
  color: #667eea;
}

.password-toggle svg {
  width: 100%;
  height: 100%;
}

/* Form Options */
.form-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.checkbox-wrapper {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  font-size: 0.875rem;
  color: #374151;
}

.checkbox-wrapper input {
  display: none;
}

.checkmark {
  width: 20px;
  height: 20px;
  border: 2px solid #d1d5db;
  border-radius: 4px;
  position: relative;
  transition: all 0.3s ease;
}

.checkbox-wrapper input:checked+.checkmark {
  background: #667eea;
  border-color: #667eea;
}

.checkbox-wrapper input:checked+.checkmark::after {
  content: '';
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 6px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: translate(-50%, -50%) rotate(45deg);
}

.forgot-link {
  font-size: 0.875rem;
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.forgot-link:hover {
  color: #5a67d8;
}

/* Buttons */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  font-weight: 600;
  border-radius: 12px;
  transition: all 0.3s ease;
  cursor: pointer;
  border: none;
  text-decoration: none;
  font-size: 1rem;
  position: relative;
  overflow: hidden;
}

.btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s;
}

.btn:hover::before {
  left: 100%;
}

.btn svg {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

.btn-primary {
  width: 100%;
  height: 56px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
  margin-bottom: 1.5rem;
}

.btn-primary:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(102, 126, 234, 0.6);
}

.btn-primary:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.loading-spinner {
  display: flex;
  align-items: center;
  justify-content: center;
}

.spinner {
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  border-top-color: white;
  animation: spin 1s ease-in-out infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Divider */
.divider {
  position: relative;
  text-align: center;
  margin: 1.5rem 0;
  color: #9ca3af;
  font-size: 0.875rem;
}

.divider::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  height: 1px;
  background: #e5e7eb;
}

.divider span {
  background: rgba(255, 255, 255, 0.95);
  padding: 0 1rem;
  position: relative;
  z-index: 1;
}

/* Social Login */
.social-login {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-bottom: 1rem;
}

.btn-social {
  height: 48px;
  background: rgba(255, 255, 255, 0.8);
  color: #374151;
  border: 1px solid #e5e7eb;
  font-size: 0.875rem;
}

.btn-social:hover {
  background: white;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.btn-social svg {
  width: 18px;
  height: 18px;
}

/* Links */
.register-link {
  margin: 0;
  color: #6b7280;
  font-size: 0.975rem;
}

.link-primary {
  color: #667eea;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;
}

.link-primary:hover {
  color: #5a67d8;
  text-decoration: underline;
}

/* Responsive Design */
@media (max-width: 640px) {
  .login-container {
    padding: 1rem;
  }

  .login-wrapper {
    max-width: 100%;
  }

  .brand-title {
    font-size: 2rem;
  }

  .form-header,
  .form-content,
  .form-footer {
    padding-left: 1.5rem;
    padding-right: 1.5rem;
  }

  .social-login {
    grid-template-columns: 1fr;
  }

  .form-options {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
}

@media (max-width: 480px) {

  .orb-1,
  .orb-2,
  .orb-3 {
    display: none;
  }
}
</style>