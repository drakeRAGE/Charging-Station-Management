<script setup>
import { RouterLink, useRouter } from 'vue-router';
import { ref, onMounted, computed } from 'vue';

const router = useRouter();
const isLoggedIn = ref(false);
const userName = ref('');
const showProfileMenu = ref(false);
const showMobileMenu = ref(false);

// Check authentication status
const checkAuth = () => {
  const token = localStorage.getItem('token');
  const user = localStorage.getItem('user');

  if (token) {
    isLoggedIn.value = true;
    if (user) {
      try {
        const userData = JSON.parse(user);
        userName.value = userData.name || userData.email || 'User';
      } catch (e) {
        userName.value = 'User';
      }
    }
  } else {
    isLoggedIn.value = false;
  }
};

// Logout function
const logout = () => {
  localStorage.removeItem('token');
  localStorage.removeItem('user');
  isLoggedIn.value = false;
  userName.value = '';
  showProfileMenu.value = false;
  router.push('/login');
};

// Toggle mobile menu
const toggleMobileMenu = () => {
  showMobileMenu.value = !showMobileMenu.value;
};

// Close mobile menu
const closeMobileMenu = () => {
  showMobileMenu.value = false;
};

// Get user initials for avatar
const userInitials = computed(() => {
  if (!userName.value) return 'U';
  return userName.value
    .split(' ')
    .map(name => name.charAt(0))
    .join('')
    .toUpperCase()
    .slice(0, 2);
});

onMounted(() => {
  checkAuth();
  // Listen for storage changes (login/logout in other tabs)
  window.addEventListener('storage', checkAuth);
});

// Close profile menu when clicking outside
const closeProfileMenu = (event) => {
  if (!event.target.closest('.profile-dropdown')) {
    showProfileMenu.value = false;
  }
};

onMounted(() => {
  document.addEventListener('click', closeProfileMenu);
});
</script>

<template>
  <nav class="navbar">
    <!-- Animated Background -->
    <div class="nav-background">
      <div class="nav-glow"></div>
      <div class="nav-pattern"></div>
    </div>

    <div class="nav-container">
      <!-- Brand Logo -->
      <RouterLink to="/" class="nav-brand" @click="closeMobileMenu">
        <div class="brand-icon">
          <div class="icon-lightning">⚡</div>
          <div class="icon-pulse"></div>
        </div>
        <span class="brand-text">ChargeHub</span>
        <div class="brand-tagline">Pro</div>
      </RouterLink>

      <!-- Desktop Navigation -->
      <div class="nav-links desktop-nav">
        <RouterLink to="/" class="nav-link">
          <span class="link-icon">🏠</span>
          <span class="link-text">Home</span>
          <div class="link-underline"></div>
        </RouterLink>

        <!-- Conditional Links for Authenticated Users -->
        <template v-if="isLoggedIn">
          <RouterLink to="/stations" class="nav-link">
            <span class="link-icon">🔌</span>
            <span class="link-text">Stations</span>
            <div class="link-underline"></div>
          </RouterLink>

        </template>

        <!-- Public Links for Non-Authenticated Users -->
        <template v-else>
          <RouterLink to="/about" class="nav-link">
            <span class="link-icon">i</span>
            <span class="link-text">About</span>
            <div class="link-underline"></div>
          </RouterLink>
        </template>
      </div>

      <!-- Authentication Section -->
      <div class="nav-auth">
        <template v-if="isLoggedIn">
          <!-- User Profile Dropdown -->
          <div class="profile-dropdown">
            <button class="profile-button" @click="showProfileMenu = !showProfileMenu"
              :class="{ active: showProfileMenu }">
              <div class="user-avatar">
                <span class="avatar-text">{{ userInitials }}</span>
                <div class="status-indicator"></div>
              </div>
              <div class="user-info">
                <span class="user-name">{{ userName }}</span>
                <span class="user-status">Online</span>
              </div>
              <div class="dropdown-arrow" :class="{ rotated: showProfileMenu }">▼</div>
            </button>

            <!-- Dropdown Menu -->
            <Transition name="dropdown">
              <div v-if="showProfileMenu" class="dropdown-menu">

                <RouterLink to="/profile" class="dropdown-item" @click="showProfileMenu = false">
                  <span class="item-icon">👤</span>
                  <span class="item-text">My Profile</span>
                </RouterLink>

                <div class="dropdown-divider"></div>

                <button class="dropdown-item logout-item" @click="logout">
                  <span class="item-icon">🚪</span>
                  <span class="item-text">Sign Out</span>
                </button>
              </div>
            </Transition>
          </div>
        </template>

        <template v-else>
          <!-- Login/Register Buttons -->
          <div class="auth-buttons">
            <RouterLink to="/register" class="btn btn-outline">
              <span class="btn-icon">✨</span>
              <span class="btn-text">Register</span>
              <div class="btn-shine"></div>
            </RouterLink>

            <RouterLink to="/login" class="btn btn-primary">
              <span class="btn-icon">🔐</span>
              <span class="btn-text">Login</span>
              <div class="btn-glow"></div>
            </RouterLink>
          </div>
        </template>

        <!-- Mobile Menu Toggle -->
        <button class="mobile-toggle" @click="toggleMobileMenu" :class="{ active: showMobileMenu }">
          <span class="toggle-line"></span>
          <span class="toggle-line"></span>
          <span class="toggle-line"></span>
        </button>
      </div>
    </div>

    <!-- Mobile Navigation Menu -->
    <Transition name="mobile-menu">
      <div v-if="showMobileMenu" class="mobile-nav">
        <div class="mobile-nav-content">
          <RouterLink to="/" class="mobile-link" @click="closeMobileMenu">
            <span class="mobile-icon">🏠</span>
            Home
          </RouterLink>

          <template v-if="isLoggedIn">
            <RouterLink to="/stations" class="mobile-link" @click="closeMobileMenu">
              <span class="mobile-icon">🔌</span>
              Stations
            </RouterLink>

            <RouterLink to="/profile" class="mobile-link" @click="closeMobileMenu">
              <span class="mobile-icon">👤</span>
              Profile
            </RouterLink>

            <button class="mobile-link logout-link" @click="logout">
              <span class="mobile-icon">🚪</span>
              Sign Out
            </button>
          </template>

          <template v-else>
            <RouterLink to="/about" class="mobile-link" @click="closeMobileMenu">
              <span class="mobile-icon">ℹ️</span>
              About
            </RouterLink>

            <RouterLink to="/contact" class="mobile-link" @click="closeMobileMenu">
              <span class="mobile-icon">📞</span>
              Contact
            </RouterLink>

            <div class="mobile-auth">
              <RouterLink to="/register" class="mobile-btn btn-outline" @click="closeMobileMenu">
                Register
              </RouterLink>
              <RouterLink to="/login" class="mobile-btn btn-primary" @click="closeMobileMenu">
                Login
              </RouterLink>
            </div>
          </template>
        </div>
      </div>
    </Transition>
  </nav>
</template>

<style scoped>
/* Base Navbar Styles */
.navbar {
  position: sticky;
  top: 0;
  z-index: 1000;
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}

.nav-background {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg,
      rgba(102, 126, 234, 0.95) 0%,
      rgba(118, 75, 162, 0.95) 100%);
  overflow: hidden;
}

.nav-glow {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 200%;
  height: 100%;
  background: radial-gradient(ellipse at center,
      rgba(255, 255, 255, 0.1) 0%,
      transparent 70%);
  animation: navGlow 4s ease-in-out infinite;
}

.nav-pattern {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image:
    radial-gradient(circle at 20% 50%, rgba(255, 255, 255, 0.05) 2px, transparent 2px),
    radial-gradient(circle at 80% 50%, rgba(255, 255, 255, 0.05) 1px, transparent 1px);
  background-size: 40px 40px;
  animation: patternMove 20s linear infinite;
}

.nav-container {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 2rem;
  max-width: 1400px;
  margin: 0 auto;
}

/* Brand Styles */
.nav-brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  color: white;
  text-decoration: none;
  font-weight: 700;
  font-size: 1.5rem;
  transition: all 0.3s ease;
}

.nav-brand:hover {
  transform: translateY(-1px);
}

.brand-icon {
  position: relative;
  width: 3rem;
  height: 3rem;
  background: linear-gradient(45deg, #00f5ff, #0080ff);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8px 25px rgba(0, 128, 255, 0.3);
}

.icon-lightning {
  font-size: 1.5rem;
  animation: bounce 2s infinite;
}

.icon-pulse {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 2px solid rgba(0, 245, 255, 0.5);
  border-radius: 50%;
  animation: pulse 2s infinite;
}

.brand-text {
  background: linear-gradient(45deg, #ffffff, #e0e7ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.brand-tagline {
  padding: 0.25rem 0.5rem;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 500;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

/* Navigation Links */
.nav-links {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.nav-link {
  position: relative;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: rgba(255, 255, 255, 0.9);
  text-decoration: none;
  padding: 0.75rem 1rem;
  border-radius: 12px;
  font-weight: 500;
  transition: all 0.3s ease;
  overflow: hidden;
}

.nav-link:hover {
  color: white;
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
}

.nav-link.router-link-active {
  color: #00f5ff;
  background: rgba(0, 245, 255, 0.1);
  border: 1px solid rgba(0, 245, 255, 0.3);
}

.link-icon {
  font-size: 1.1rem;
  transition: transform 0.3s ease;
}

.nav-link:hover .link-icon {
  transform: scale(1.2);
}

.link-underline {
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #00f5ff, #0080ff);
  transition: all 0.3s ease;
  transform: translateX(-50%);
}

.nav-link:hover .link-underline {
  width: 80%;
}

/* Authentication Section */
.nav-auth {
  display: flex;
  align-items: center;
  gap: 1rem;
}

/* Profile Dropdown */
.profile-dropdown {
  position: relative;
}

.profile-button {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  padding: 0.5rem 1rem;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.profile-button:hover,
.profile-button.active {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-1px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.user-avatar {
  position: relative;
  width: 2.5rem;
  height: 2.5rem;
  background: linear-gradient(45deg, #ff6b6b, #ffa500);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.9rem;
}

.status-indicator {
  position: absolute;
  bottom: 2px;
  right: 2px;
  width: 8px;
  height: 8px;
  background: #00ff00;
  border-radius: 50%;
  border: 2px solid white;
  animation: statusPulse 2s infinite;
}

.user-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.user-name {
  font-weight: 600;
  font-size: 0.9rem;
}

.user-status {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.7);
}

.dropdown-arrow {
  font-size: 0.8rem;
  transition: transform 0.3s ease;
}

.dropdown-arrow.rotated {
  transform: rotate(180deg);
}

/* Dropdown Menu */
.dropdown-menu {
  position: absolute;
  top: calc(100% + 0.5rem);
  right: 0;
  min-width: 280px;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  z-index: 1000;
}

.dropdown-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1.5rem;
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1));
}

.header-avatar {
  width: 3rem;
  height: 3rem;
  background: linear-gradient(45deg, #ff6b6b, #ffa500);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: 600;
  font-size: 1.1rem;
}

.header-info {
  flex: 1;
}

.header-name {
  font-weight: 600;
  color: #333;
  margin-bottom: 0.25rem;
}

.header-email {
  font-size: 0.8rem;
  color: #666;
}

.dropdown-divider {
  height: 1px;
  background: rgba(0, 0, 0, 0.1);
  margin: 0.5rem 0;
}

.dropdown-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1.5rem;
  color: #333;
  text-decoration: none;
  transition: all 0.2s ease;
  border: none;
  background: none;
  width: 100%;
  cursor: pointer;
  font-size: 0.9rem;
}

.dropdown-item:hover {
  background: rgba(102, 126, 234, 0.1);
  color: #667eea;
}

.logout-item:hover {
  background: rgba(255, 107, 107, 0.1);
  color: #ff6b6b;
}

.item-icon {
  font-size: 1rem;
  width: 1.2rem;
  text-align: center;
}

/* Auth Buttons */
.auth-buttons {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.btn {
  position: relative;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border-radius: 12px;
  text-decoration: none;
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.3s ease;
  overflow: hidden;
  border: none;
  cursor: pointer;
}

.btn-outline {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(10px);
}

.btn-outline:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(255, 255, 255, 0.1);
}

.btn-primary {
  background: linear-gradient(45deg, #00f5ff, #0080ff);
  color: white;
  box-shadow: 0 8px 25px rgba(0, 128, 255, 0.3);
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 35px rgba(0, 128, 255, 0.4);
}

.btn-glow {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
  transition: left 0.5s;
}

.btn-primary:hover .btn-glow {
  left: 100%;
}

.btn-shine {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s;
}

.btn-outline:hover .btn-shine {
  left: 100%;
}

/* Mobile Styles */
.mobile-toggle {
  display: none;
  flex-direction: column;
  gap: 4px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.toggle-line {
  width: 24px;
  height: 2px;
  background: white;
  border-radius: 2px;
  transition: all 0.3s ease;
}

.mobile-toggle.active .toggle-line:nth-child(1) {
  transform: rotate(45deg) translate(6px, 6px);
}

.mobile-toggle.active .toggle-line:nth-child(2) {
  opacity: 0;
}

.mobile-toggle.active .toggle-line:nth-child(3) {
  transform: rotate(-45deg) translate(6px, -6px);
}

.mobile-nav {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: rgba(102, 126, 234, 0.98);
  backdrop-filter: blur(20px);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.mobile-nav-content {
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.mobile-link {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem;
  color: white;
  text-decoration: none;
  border-radius: 12px;
  font-weight: 500;
  transition: all 0.3s ease;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  width: 100%;
  text-align: left;
}

.mobile-link:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateX(8px);
}

.mobile-icon {
  font-size: 1.2rem;
  width: 1.5rem;
  text-align: center;
}

.logout-link {
  color: #ff6b6b;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  margin-top: 1rem;
  padding-top: 1.5rem;
}

.mobile-auth {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.mobile-btn {
  flex: 1;
  text-align: center;
  padding: 1rem;
  border-radius: 12px;
  text-decoration: none;
  font-weight: 600;
  transition: all 0.3s ease;
}

/* Responsive Design */
@media (max-width: 768px) {
  .nav-container {
    padding: 1rem;
  }

  .desktop-nav {
    display: none;
  }

  .mobile-toggle {
    display: flex;
  }

  .auth-buttons {
    display: none;
  }

  .profile-button {
    padding: 0.5rem;
  }

  .user-info {
    display: none;
  }

  .dropdown-menu {
    right: 0;
    min-width: 250px;
  }
}

@media (max-width: 480px) {
  .nav-container {
    padding: 0.75rem;
  }

  .brand-text {
    display: none;
  }

  .brand-tagline {
    display: none;
  }
}

/* Animations */
@keyframes navGlow {

  0%,
  100% {
    opacity: 0.3;
    transform: translateX(-50%) scale(1);
  }

  50% {
    opacity: 0.6;
    transform: translateX(-50%) scale(1.1);
  }
}

@keyframes patternMove {
  0% {
    transform: translateX(0);
  }

  100% {
    transform: translateX(40px);
  }
}

@keyframes bounce {

  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translateY(0);
  }

  40% {
    transform: translateY(-4px);
  }

  60% {
    transform: translateY(-2px);
  }
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 1;
  }

  100% {
    transform: scale(1.4);
    opacity: 0;
  }
}

@keyframes statusPulse {

  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.5;
  }
}

/* Transitions */
.dropdown-enter-active,
.dropdown-leave-active {
  transition: all 0.3s ease;
}

.dropdown-enter-from,
.dropdown-leave-to {
  opacity: 0;
  transform: translateY(-10px) scale(0.95);
}

.mobile-menu-enter-active,
.mobile-menu-leave-active {
  transition: all 0.3s ease;
}

.mobile-menu-enter-from,
.mobile-menu-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

/* Hide scrollbar for better mobile experience */
.dropdown-menu {
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.dropdown-menu::-webkit-scrollbar {
  display: none;
}
</style>