<script setup>
import { ref, onMounted } from 'vue';

// Animation states
const isVisible = ref(false);
const currentTestimonial = ref(0);
const currentStat = ref(0);

// Team data
const team = ref([
    {
        name: 'XYZ Johnson',
        role: 'CEO & Founder',
        image: '👩‍💼',
        bio: 'Visionary leader with 15+ years in sustainable technology',
        linkedin: '#'
    },
    {
        name: 'ABC Chen',
        role: 'CTO',
        image: '👨‍💻',
        bio: 'Tech innovator specializing in EV infrastructure',
        linkedin: '#'
    },
    {
        name: 'DEF Rodriguez',
        role: 'Head of Operations',
        image: '👩‍🔧',
        bio: 'Operations expert ensuring seamless user experience',
        linkedin: '#'
    },
    {
        name: 'JKL Kim',
        role: 'Lead Engineer',
        image: '👨‍🔬',
        bio: 'Engineering wizard building the future of charging',
        linkedin: '#'
    }
]);

// Testimonials data
const testimonials = ref([
    {
        name: 'MNO Thompson',
        role: 'Tesla Owner',
        content: 'ChargeHub has revolutionized my EV experience. Finding charging stations has never been easier!',
        rating: 5,
        avatar: '👨‍💼'
    },
    {
        name: 'IJK Park',
        role: 'Fleet Manager',
        content: 'Managing our company fleet is seamless with ChargeHub\'s comprehensive platform.',
        rating: 5,
        avatar: '👩‍💼'
    },
    {
        name: 'RST Wilson',
        role: 'Long-distance Driver',
        content: 'The route planning feature saved me hours on my cross-country trip. Absolutely brilliant!',
        rating: 5,
        avatar: '🚗'
    }
]);

// Timeline data
const milestones = ref([
    {
        year: '2020',
        title: 'Company Founded',
        description: 'Started with a vision to revolutionize EV charging infrastructure',
        icon: '🚀'
    },
    {
        year: '2021',
        title: 'First 1,000 Stations',
        description: 'Reached our first major milestone of partnering with 1,000 charging stations',
        icon: '⚡'
    },
    {
        year: '2022',
        title: 'Mobile App Launch',
        description: 'Launched our award-winning mobile application for iOS and Android',
        icon: '📱'
    },
    {
        year: '2023',
        title: 'AI Integration',
        description: 'Introduced AI-powered route optimization and predictive availability',
        icon: '🤖'
    },
    {
        year: '2024',
        title: 'Global Expansion',
        description: 'Expanded to 500+ cities worldwide with premium charging solutions',
        icon: '🌍'
    }
]);

// Lifecycle hooks
onMounted(() => {
    isVisible.value = true;

    // Auto-rotate testimonials
    setInterval(() => {
        currentTestimonial.value = (currentTestimonial.value + 1) % testimonials.value.length;
    }, 5000);

});

// Methods
const selectTestimonial = (index) => {
    currentTestimonial.value = index;
};

const renderStars = (rating) => {
    return '⭐'.repeat(rating);
};
</script>

<template>
    <div class="about-page">
        <!-- Animated Background -->
        <div class="background-animation">
            <div class="floating-orb orb-1"></div>
            <div class="floating-orb orb-2"></div>
            <div class="floating-orb orb-3"></div>
            <div class="grid-pattern"></div>
        </div>

        <!-- Hero Section -->
        <section class="hero-section">
            <div class="container">
                <div class="hero-content" :class="{ visible: isVisible }">
                    <div class="hero-badge">
                        <span class="badge-icon">⚡</span>
                        About ChargeHub
                    </div>

                    <h1 class="hero-title">
                        <span class="title-gradient">Powering the Future</span>
                        <br>
                        <span class="title-accent">of Electric Mobility</span>
                    </h1>

                    <p class="hero-description">
                        We're on a mission to accelerate the world's transition to sustainable transportation
                        by making electric vehicle charging accessible, reliable, and intelligent for everyone.
                    </p>
                </div>

                <div class="hero-visual">
                    <div class="charging-hub">
                        <div class="hub-base"></div>
                        <div class="hub-stations">
                            <div class="station" v-for="i in 4" :key="i" :style="{ animationDelay: i * 0.5 + 's' }">
                                <div class="station-pillar"></div>
                                <div class="station-light"></div>
                            </div>
                        </div>
                        <div class="energy-flow">
                            <div class="flow-line" v-for="i in 8" :key="i" :style="{ animationDelay: i * 0.3 + 's' }">
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Mission Section -->
        <section class="mission-section">
            <div class="container">
                <div class="section-header">
                    <h2 class="section-title">Our Mission</h2>
                    <p class="section-subtitle">Building a sustainable future through innovative technology</p>
                </div>

                <div class="mission-grid">
                    <div class="mission-card">
                        <div class="card-icon">🌱</div>
                        <h3>Sustainability First</h3>
                        <p>Every decision we make prioritizes environmental impact and sustainable practices for a
                            greener tomorrow.</p>
                    </div>

                    <div class="mission-card">
                        <div class="card-icon">🚀</div>
                        <h3>Innovation Driven</h3>
                        <p>We leverage cutting-edge technology and AI to create the most intelligent charging solutions
                            available.</p>
                    </div>

                    <div class="mission-card">
                        <div class="card-icon">🤝</div>
                        <h3>Community Focused</h3>
                        <p>Building strong partnerships with businesses and communities to create accessible charging
                            networks.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Timeline Section -->
        <section class="timeline-section">
            <div class="container">
                <div class="section-header">
                    <h2 class="section-title">Our Journey</h2>
                    <p class="section-subtitle">Key milestones in our mission to transform EV charging</p>
                </div>

                <div class="timeline">
                    <div class="timeline-item" v-for="(milestone, index) in milestones" :key="index">
                        <div class="timeline-dot">
                            <span class="dot-icon">{{ milestone.icon }}</span>
                        </div>
                        <div class="timeline-content">
                            <div class="timeline-year">{{ milestone.year }}</div>
                            <h3 class="timeline-title">{{ milestone.title }}</h3>
                            <p class="timeline-description">{{ milestone.description }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Team Section -->
        <section class="team-section">
            <div class="container">
                <div class="section-header">
                    <h2 class="section-title">Meet Our Team</h2>
                    <p class="section-subtitle">The brilliant minds behind ChargeHub's success</p>
                </div>

                <div class="team-grid">
                    <div class="team-card" v-for="(member, index) in team" :key="index">
                        <div class="member-avatar">
                            <span class="avatar-emoji">{{ member.image }}</span>
                            <div class="avatar-ring"></div>
                        </div>
                        <div class="member-info">
                            <h3 class="member-name">{{ member.name }}</h3>
                            <p class="member-role">{{ member.role }}</p>
                            <p class="member-bio">{{ member.bio }}</p>
                            <a :href="member.linkedin" class="linkedin-link">
                                <span class="link-icon">💼</span>
                                Connect
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Testimonials Section -->
        <section class="testimonials-section">
            <div class="container">
                <div class="section-header">
                    <h2 class="section-title">What Our Users Say</h2>
                    <p class="section-subtitle">Real feedback from our amazing community</p>
                </div>

                <div class="testimonials-container">
                    <div class="testimonial-card" v-for="(testimonial, index) in testimonials" :key="index"
                        :class="{ active: index === currentTestimonial }">
                        <div class="testimonial-content">
                            <div class="quote-icon">"</div>
                            <p class="testimonial-text">{{ testimonial.content }}</p>
                            <div class="testimonial-rating">{{ renderStars(testimonial.rating) }}</div>
                        </div>
                        <div class="testimonial-author">
                            <div class="author-avatar">{{ testimonial.avatar }}</div>
                            <div class="author-info">
                                <div class="author-name">{{ testimonial.name }}</div>
                                <div class="author-role">{{ testimonial.role }}</div>
                            </div>
                        </div>
                    </div>

                    <div class="testimonial-dots">
                        <button v-for="(testimonial, index) in testimonials" :key="index" class="dot"
                            :class="{ active: index === currentTestimonial }"
                            @click="selectTestimonial(index)"></button>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<style scoped>
/* Global Styles */
.about-page {
    position: relative;
    min-height: 100vh;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    overflow-x: hidden;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem;
}

/* Background Animation */
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
    width: 300px;
    height: 300px;
    top: 10%;
    left: 5%;
    animation-delay: 0s;
}

.orb-2 {
    width: 200px;
    height: 200px;
    top: 50%;
    right: 10%;
    animation-delay: 3s;
}

.orb-3 {
    width: 150px;
    height: 150px;
    bottom: 20%;
    left: 60%;
    animation-delay: 6s;
}

.grid-pattern {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image:
        radial-gradient(circle at 25% 25%, rgba(255, 255, 255, 0.03) 2px, transparent 2px),
        radial-gradient(circle at 75% 75%, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
    background-size: 80px 80px;
    animation: gridMove 30s linear infinite;
}

/* Hero Section */
.hero-section {
    position: relative;
    z-index: 10;
    padding: 6rem 0;
    min-height: 100vh;
    display: flex;
    align-items: center;
}

.hero-content {
    opacity: 0;
    transform: translateY(50px);
    transition: all 1s ease;
}

.hero-content.visible {
    opacity: 1;
    transform: translateY(0);
}

.hero-badge {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.75rem 1.5rem;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 50px;
    font-size: 0.9rem;
    margin-bottom: 2rem;
    backdrop-filter: blur(10px);
}

.badge-icon {
    animation: pulse 2s infinite;
}

.hero-title {
    font-size: 4rem;
    font-weight: 800;
    line-height: 1.1;
    margin-bottom: 2rem;
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
    margin-bottom: 3rem;
    color: rgba(255, 255, 255, 0.8);
    max-width: 600px;
}

.hero-stats {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
    margin-top: 4rem;
}

.stat-item {
    text-align: center;
    padding: 1.5rem;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    transform: translateY(20px);
    opacity: 0;
    transition: all 0.6s ease;
}

.stat-item.animated {
    transform: translateY(0);
    opacity: 1;
}

.stat-icon {
    font-size: 2rem;
    margin-bottom: 0.5rem;
}

.stat-number {
    font-size: 2.5rem;
    font-weight: 800;
    color: #00f5ff;
    margin-bottom: 0.5rem;
}

.stat-label {
    font-size: 0.9rem;
    color: rgba(255, 255, 255, 0.7);
}

/* Hero Visual */
.hero-visual {
    position: absolute;
    top: 50%;
    right: 5%;
    transform: translateY(-50%);
    z-index: 5;
}

.charging-hub {
    position: relative;
    width: 300px;
    height: 300px;
}

.hub-base {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    width: 200px;
    height: 30px;
    background: linear-gradient(45deg, #333, #555);
    border-radius: 15px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.hub-stations {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 200px;
    height: 200px;
}

.station {
    position: absolute;
    width: 20px;
    height: 60px;
    animation: stationPulse 2s ease-in-out infinite;
}

.station:nth-child(1) {
    top: 0;
    left: 50%;
    transform: translateX(-50%);
}

.station:nth-child(2) {
    right: 0;
    top: 50%;
    transform: translateY(-50%);
}

.station:nth-child(3) {
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
}

.station:nth-child(4) {
    left: 0;
    top: 50%;
    transform: translateY(-50%);
}

.station-pillar {
    width: 100%;
    height: 80%;
    background: linear-gradient(180deg, #444, #222);
    border-radius: 4px;
}

.station-light {
    position: absolute;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    width: 6px;
    height: 6px;
    background: #00ff00;
    border-radius: 50%;
    animation: lightBlink 1s ease-in-out infinite;
}

.energy-flow {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100px;
    height: 100px;
}

.flow-line {
    position: absolute;
    width: 2px;
    height: 20px;
    background: linear-gradient(180deg, transparent, #00f5ff, transparent);
    border-radius: 1px;
    animation: flowAnimation 2s linear infinite;
}

/* Section Styles */
section {
    position: relative;
    z-index: 10;
    padding: 5rem 0;
}

.section-header {
    text-align: center;
    margin-bottom: 4rem;
}

.section-title {
    font-size: 3rem;
    font-weight: 700;
    margin-bottom: 1rem;
}

.section-subtitle {
    font-size: 1.2rem;
    color: rgba(255, 255, 255, 0.7);
    max-width: 600px;
    margin: 0 auto;
}

/* Mission Section */
.mission-section {
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(10px);
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.mission-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
}

.mission-card {
    text-align: center;
    padding: 3rem 2rem;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 20px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
}

.mission-card:hover {
    transform: translateY(-10px);
    background: rgba(255, 255, 255, 0.15);
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2);
}

.card-icon {
    font-size: 4rem;
    margin-bottom: 1.5rem;
    display: block;
}

.mission-card h3 {
    font-size: 1.5rem;
    font-weight: 600;
    margin-bottom: 1rem;
}

.mission-card p {
    color: rgba(255, 255, 255, 0.8);
    line-height: 1.6;
}

/* Timeline Section */
.timeline {
    position: relative;
    max-width: 800px;
    margin: 0 auto;
}

.timeline::before {
    content: '';
    position: absolute;
    left: 50%;
    top: 0;
    bottom: 0;
    width: 2px;
    background: linear-gradient(180deg, #00f5ff, #0080ff);
    transform: translateX(-50%);
}

.timeline-item {
    position: relative;
    display: flex;
    margin-bottom: 3rem;
}

.timeline-item:nth-child(odd) {
    flex-direction: row-reverse;
}

.timeline-dot {
    position: absolute;
    left: 50%;
    top: 20px;
    transform: translateX(-50%);
    width: 60px;
    height: 60px;
    background: linear-gradient(45deg, #00f5ff, #0080ff);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    box-shadow: 0 10px 30px rgba(0, 128, 255, 0.3);
    z-index: 10;
}

.timeline-content {
    flex: 1;
    max-width: calc(50% - 40px);
    padding: 1.5rem;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    margin: 0 40px;
}

.timeline-year {
    font-size: 1.2rem;
    font-weight: 700;
    color: #00f5ff;
    margin-bottom: 0.5rem;
}

.timeline-title {
    font-size: 1.3rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
}

.timeline-description {
    color: rgba(255, 255, 255, 0.8);
    line-height: 1.5;
}

/* Team Section */
.team-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 2rem;
}

.team-card {
    text-align: center;
    padding: 2rem;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 20px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
}

.team-card:hover {
    transform: translateY(-5px);
    background: rgba(255, 255, 255, 0.15);
}

.member-avatar {
    position: relative;
    width: 100px;
    height: 100px;
    margin: 0 auto 1.5rem;
    background: linear-gradient(45deg, #ff6b6b, #ffa500);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 3rem;
}

.avatar-ring {
    position: absolute;
    width: 120px;
    height: 120px;
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-radius: 50%;
    animation: rotate 10s linear infinite;
}

.member-name {
    font-size: 1.3rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
}

.member-role {
    color: #00f5ff;
    font-weight: 500;
    margin-bottom: 1rem;
}

.member-bio {
    color: rgba(255, 255, 255, 0.8);
    line-height: 1.5;
    margin-bottom: 1.5rem;
}

.linkedin-link {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    color: white;
    text-decoration: none;
    padding: 0.5rem 1rem;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 8px;
    transition: all 0.3s ease;
}

.linkedin-link:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: translateY(-2px);
}

/* Testimonials Section */
.testimonials-section {
    background: rgba(0, 0, 0, 0.2);
    backdrop-filter: blur(10px);
}

.testimonials-container {
    position: relative;
    max-width: 800px;
    margin: 0 auto;
}

.testimonial-card {
    opacity: 0;
    transform: scale(0.9);
    transition: all 0.5s ease;
    position: absolute;
    width: 100%;
    padding: 3rem;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 20px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    backdrop-filter: blur(10px);
}

.testimonial-card.active {
    opacity: 1;
    transform: scale(1);
    position: relative;
}

.testimonial-content {
    text-align: center;
    margin-bottom: 2rem;
}

.quote-icon {
    font-size: 4rem;
    color: rgba(255, 255, 255, 0.3);
    line-height: 1;
    margin-bottom: 1rem;
}

.testimonial-text {
    font-size: 1.2rem;
    line-height: 1.6;
    margin-bottom: 1rem;
    color: rgba(255, 255, 255, 0.9);
}

.testimonial-rating {
    font-size: 1.5rem;
    margin-bottom: 1rem;
}

.testimonial-author {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
}

.author-avatar {
    width: 60px;
    height: 60px;
    background: linear-gradient(45deg, #00f5ff, #0080ff);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.8rem;
}

.author-name {
    font-weight: 600;
    font-size: 1.1rem;
}

.author-role {
    color: rgba(255, 255, 255, 0.7);
    font-size: 0.9rem;
}

.testimonial-dots {
    display: flex;
    justify-content: center;
    gap: 1rem;
    margin-top: 2rem;
}

.dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.3);
    border: none;
    transition: all 0.3s ease;
}
</style>
