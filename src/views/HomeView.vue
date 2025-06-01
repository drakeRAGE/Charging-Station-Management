<template>
  <div class="home">
    <!-- Animated Background -->
    <div class="background-animation">
      <div class="floating-orb orb-1"></div>
      <div class="floating-orb orb-2"></div>
      <div class="floating-orb orb-3"></div>
      <div class="charging-grid"></div>
    </div>

    <!-- Hero Section with 3D Elements -->
    <div class="hero">
      <div class="hero-content">
        <div class="hero-badge">
          <span class="badge-glow">⚡</span>
          Next Generation EV Infrastructure
        </div>

        <h1 class="hero-title">
          <span class="title-gradient">Power Your Journey</span>
          <br>
          <span class="title-accent">With Smart Charging</span>
        </h1>

        <p class="hero-description">
          Discover, navigate, and manage electric vehicle charging stations
          with our cutting-edge platform. Join the sustainable transportation revolution.
        </p>

        <div class="hero-actions">
          <router-link to="/stations" class="btn btn-primary">
            <span class="btn-icon">🚀</span>
            Explore Stations
            <div class="btn-glow"></div>
          </router-link>
        </div>

      </div>

      <!-- 3D Charging Station Visualization -->
      <div class="hero-visual">
        <div class="charging-station">
          <div class="station-base"></div>
          <div class="station-pillar">
            <div class="led-strip"></div>
            <div class="screen">
              <div class="screen-content">
                <div class="charging-animation"></div>
              </div>
            </div>
          </div>
          <div class="charging-cable"></div>
        </div>

        <!-- Floating Elements -->
        <div class="floating-elements">
          <div class="element element-1">⚡</div>
          <div class="element element-2">🔋</div>
          <div class="element element-3">🌱</div>
          <div class="element element-4">♻️</div>
        </div>
      </div>
    </div>

    <!-- Features Section -->
    <div class="features-section">
      <div class="section-header">
        <h2 class="section-title">Why Choose ChargeHub?</h2>
        <p class="section-subtitle">Advanced features for the modern EV driver</p>
      </div>

      <div class="features-grid">
        <div class="feature-card">
          <div class="feature-icon">🗺️</div>
          <h3>Smart Navigation</h3>
          <p>AI-powered route optimization with real-time availability</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">💳</div>
          <h3>Seamless Payment</h3>
          <p>One-tap payment with multiple options and billing history</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">📊</div>
          <h3>Usage Analytics</h3>
          <p>Track your charging patterns and carbon footprint savings</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  mounted() {
    this.initAnimations();
  },
  methods: {
    initAnimations() {
      // Add any additional JavaScript animations here
      const floatingElements = document.querySelectorAll('.floating-elements .element');
      floatingElements.forEach((element, index) => {
        element.style.animationDelay = `${index * 0.5}s`;
      });
    }
  }
}
</script>

<style scoped>
/* Global Styles */
.home {
  position: relative;
  min-height: 100vh;
  overflow: hidden;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}

/* Animated Background */
.background-animation {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}

.floating-orb {
  position: absolute;
  border-radius: 50%;
  background: linear-gradient(45deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
  backdrop-filter: blur(10px);
  animation: float 8s ease-in-out infinite;
}

.orb-1 {
  width: 200px;
  height: 200px;
  top: 10%;
  left: 10%;
  animation-delay: 0s;
}

.orb-2 {
  width: 150px;
  height: 150px;
  top: 60%;
  right: 15%;
  animation-delay: 3s;
}

.orb-3 {
  width: 100px;
  height: 100px;
  bottom: 20%;
  left: 50%;
  animation-delay: 6s;
}

.charging-grid {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image:
    radial-gradient(circle at 25% 25%, rgba(255, 255, 255, 0.05) 2px, transparent 2px),
    radial-gradient(circle at 75% 75%, rgba(255, 255, 255, 0.05) 1px, transparent 1px);
  background-size: 60px 60px;
  animation: gridMove 20s linear infinite;
}

/* Glass Navigation */
.navbar {
  position: relative;
  z-index: 10;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 40px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  margin: 20px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.nav-brand {
  display: flex;
  align-items: center;
  gap: 12px;
  font-weight: 700;
  font-size: 1.5rem;
}

.brand-icon {
  width: 40px;
  height: 40px;
  background: linear-gradient(45deg, #00f5ff, #0080ff);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  animation: pulse 2s infinite;
}

.nav-links {
  display: flex;
  gap: 30px;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 20px;
  color: white;
  text-decoration: none;
  border-radius: 12px;
  transition: all 0.3s ease;
  font-weight: 500;
}

.nav-link:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

.nav-icon {
  font-size: 1.1rem;
}

/* Hero Section */
.hero {
  position: relative;
  z-index: 5;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
  padding: 80px 40px;
  min-height: 80vh;
}

.hero-content {
  max-width: 600px;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 8px 20px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50px;
  font-size: 0.9rem;
  margin-bottom: 30px;
  backdrop-filter: blur(10px);
}

.badge-glow {
  animation: glow 2s ease-in-out infinite alternate;
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 800;
  line-height: 1.1;
  margin-bottom: 30px;
}

.title-gradient {
  background: linear-gradient(45deg, #00f5ff, #ff6b6b);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.title-accent {
  color: rgba(255, 255, 255, 0.9);
}

.hero-description {
  font-size: 1.3rem;
  line-height: 1.6;
  margin-bottom: 40px;
  color: rgba(255, 255, 255, 0.8);
}

.hero-actions {
  display: flex;
  gap: 20px;
  margin-bottom: 60px;
}

.btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 16px 32px;
  border: none;
  border-radius: 12px;
  font-size: 1.1rem;
  font-weight: 600;
  text-decoration: none;
  cursor: pointer;
  transition: all 0.3s ease;
  overflow: hidden;
}

.btn-primary {
  background: linear-gradient(45deg, #00f5ff, #0080ff);
  color: white;
  box-shadow: 0 10px 30px rgba(0, 128, 255, 0.3);
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 15px 40px rgba(0, 128, 255, 0.4);
}

.btn-secondary {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(10px);
}

.btn-secondary:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
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

/* Stats Grid */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
}

.stat-card {
  text-align: center;
  padding: 20px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.stat-number {
  font-size: 2rem;
  font-weight: 800;
  color: #00f5ff;
  margin-bottom: 5px;
}

.stat-label {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.7);
}

/* Hero Visual */
.hero-visual {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.charging-station {
  position: relative;
  transform: perspective(1000px) rotateY(-15deg);
  animation: float 6s ease-in-out infinite;
}

.station-base {
  width: 120px;
  height: 20px;
  background: linear-gradient(45deg, #333, #555);
  border-radius: 10px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.station-pillar {
  width: 80px;
  height: 200px;
  background: linear-gradient(180deg, #444, #222);
  border-radius: 10px;
  margin: 0 auto;
  position: relative;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
}

.led-strip {
  position: absolute;
  right: 10px;
  top: 20px;
  width: 4px;
  height: 160px;
  background: linear-gradient(180deg, #00ff00, #ffff00, #ff0000);
  border-radius: 2px;
  animation: ledPulse 2s ease-in-out infinite;
}

.screen {
  position: absolute;
  left: 10px;
  top: 40px;
  width: 50px;
  height: 80px;
  background: #000;
  border-radius: 8px;
  border: 2px solid #333;
  overflow: hidden;
}

.screen-content {
  padding: 10px;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.charging-animation {
  width: 20px;
  height: 30px;
  background: linear-gradient(180deg, transparent 0%, #00ff00 50%, transparent 100%);
  animation: chargingBar 1.5s ease-in-out infinite;
}

.charging-cable {
  position: absolute;
  bottom: -20px;
  left: 30px;
  width: 20px;
  height: 40px;
  background: #333;
  border-radius: 0 0 10px 10px;
}

/* Floating Elements */
.floating-elements {
  position: absolute;
  width: 400px;
  height: 400px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.element {
  position: absolute;
  font-size: 2rem;
  animation: floatAround 8s ease-in-out infinite;
}

.element-1 {
  top: 20%;
  left: 10%;
  animation-delay: 0s;
}

.element-2 {
  top: 10%;
  right: 20%;
  animation-delay: 2s;
}

.element-3 {
  bottom: 20%;
  left: 20%;
  animation-delay: 4s;
}

.element-4 {
  bottom: 10%;
  right: 10%;
  animation-delay: 6s;
}

/* Features Section */
.features-section {
  position: relative;
  z-index: 5;
  padding: 80px 40px;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  margin: 40px;
  border-radius: 30px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.section-header {
  text-align: center;
  margin-bottom: 60px;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 20px;
}

.section-subtitle {
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 0.7);
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 40px;
}

.feature-card {
  text-align: center;
  padding: 40px 30px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.feature-card:hover {
  transform: translateY(-10px);
  background: rgba(255, 255, 255, 0.15);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2);
}

.feature-icon {
  font-size: 3rem;
  margin-bottom: 20px;
  display: block;
}

.feature-card h3 {
  font-size: 1.4rem;
  font-weight: 600;
  margin-bottom: 15px;
}

.feature-card p {
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.6;
}

/* Animations */
@keyframes float {

  0%,
  100% {
    transform: translateY(0px) rotate(0deg);
  }

  33% {
    transform: translateY(-20px) rotate(1deg);
  }

  66% {
    transform: translateY(-10px) rotate(-1deg);
  }
}

@keyframes floatAround {

  0%,
  100% {
    transform: translate(0, 0) rotate(0deg);
  }

  25% {
    transform: translate(30px, -30px) rotate(90deg);
  }

  50% {
    transform: translate(-20px, -40px) rotate(180deg);
  }

  75% {
    transform: translate(-40px, 20px) rotate(270deg);
  }
}

@keyframes pulse {

  0%,
  100% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.1);
  }
}

@keyframes glow {
  0% {
    text-shadow: 0 0 5px rgba(0, 245, 255, 0.5);
  }

  100% {
    text-shadow: 0 0 20px rgba(0, 245, 255, 0.8);
  }
}

@keyframes gridMove {
  0% {
    transform: translate(0, 0);
  }

  100% {
    transform: translate(60px, 60px);
  }
}

@keyframes ledPulse {

  0%,
  100% {
    opacity: 0.5;
  }

  50% {
    opacity: 1;
  }
}

@keyframes chargingBar {
  0% {
    height: 10%;
  }

  50% {
    height: 80%;
  }

  100% {
    height: 10%;
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .hero {
    grid-template-columns: 1fr;
    text-align: center;
    padding: 40px 20px;
  }

  .hero-title {
    font-size: 2.5rem;
  }

  .stats-grid {
    grid-template-columns: 1fr;
  }

  .navbar {
    flex-direction: column;
    gap: 20px;
    margin: 10px;
    padding: 20px;
  }

  .hero-actions {
    flex-direction: column;
    align-items: center;
  }

  .features-grid {
    grid-template-columns: 1fr;
  }

  .features-section {
    margin: 20px;
    padding: 40px 20px;
  }
}
</style>