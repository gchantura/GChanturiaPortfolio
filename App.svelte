<script>
  import { onMount } from 'svelte';
  
  // State
  let scrollY = $state(0);
  let activeSection = $state('home');
  let mobileMenuOpen = $state(false);
  let mounted = $state(false);
  let languageDropdownOpen = $state(false);
  let expandedCard = $state(null);
  let formData = $state({ name: '', email: '', message: '' });
  let formStatus = $state('idle');
  
  // Typing effect state
  let typedText = $state('');
  const fullText = 'Systems Engineering & Cloud Architecture';
  let typeIndex = $state(0);
  
  // Navigation items
  const navItems = [
    { id: 'home', label: 'Home' },
    { id: 'systems', label: 'Systems' },
    { id: 'resume', label: 'Resume' },
    { id: 'contact', label: 'Contact' }
  ];
  
  // Languages
  const languages = [
    { code: 'ka', name: 'Georgian', level: 'Native', flag: '🇬🇪' },
    { code: 'en', name: 'English', level: 'C1', flag: '🇬🇧' },
    { code: 'ru', name: 'Russian', level: 'C1', flag: '🇷🇺' }
  ];
  
  // Systems cards data
  const systemsCards = [
    {
      id: 'infrastructure',
      title: 'Cloud Infrastructure',
      subtitle: 'Scalable & Resilient',
      description: 'Designing fault-tolerant distributed systems with multi-region deployments, auto-scaling policies, and infrastructure as code.',
      technologies: ['AWS', 'Terraform', 'Kubernetes', 'Docker'],
      codeSnippet: `resource "aws_eks_cluster" "main" {
  name     = "production-cluster"
  role_arn = aws_iam_role.eks.arn
  
  vpc_config {
    subnet_ids = var.private_subnets
  }
}`
    },
    {
      id: 'devops',
      title: 'DevOps & CI/CD',
      subtitle: 'Automated Pipelines',
      description: 'Building robust deployment pipelines with automated testing, security scanning, and zero-downtime deployments.',
      technologies: ['GitHub Actions', 'ArgoCD', 'Helm', 'Prometheus'],
      codeSnippet: `name: Deploy Production
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: helm upgrade --install app ./chart`
    },
    {
      id: 'security',
      title: 'Security & Compliance',
      subtitle: 'Zero Trust Architecture',
      description: 'Implementing defense-in-depth strategies with identity management, encryption, and continuous security monitoring.',
      technologies: ['Vault', 'IAM', 'mTLS', 'SIEM'],
      codeSnippet: `policy "production-access" {
  path "secret/data/prod/*" {
    capabilities = ["read"]
    required_parameters = ["reason"]
  }
}`
    }
  ];
  
  // Skills data
  const skills = [
    { name: 'Cloud Platforms', items: ['AWS', 'GCP', 'Azure'] },
    { name: 'Infrastructure', items: ['Terraform', 'Pulumi', 'CloudFormation'] },
    { name: 'Containers', items: ['Kubernetes', 'Docker', 'ECS'] },
    { name: 'CI/CD', items: ['GitHub Actions', 'GitLab CI', 'Jenkins'] },
    { name: 'Monitoring', items: ['Prometheus', 'Grafana', 'Datadog'] },
    { name: 'Languages', items: ['Python', 'Go', 'TypeScript'] }
  ];
  
  onMount(() => {
    mounted = true;
    
    // Typing effect
    const typeInterval = setInterval(() => {
      if (typeIndex < fullText.length) {
        typedText = fullText.slice(0, typeIndex + 1);
        typeIndex++;
      } else {
        clearInterval(typeInterval);
      }
    }, 50);
    
    // Scroll handler
    const handleScroll = () => {
      scrollY = window.scrollY;
      
      // Update active section based on scroll position
      const sections = ['home', 'systems', 'resume', 'contact'];
      for (const section of sections) {
        const el = document.getElementById(section);
        if (el) {
          const rect = el.getBoundingClientRect();
          if (rect.top <= 100 && rect.bottom >= 100) {
            activeSection = section;
            break;
          }
        }
      }
    };
    
    window.addEventListener('scroll', handleScroll);
    
    return () => {
      clearInterval(typeInterval);
      window.removeEventListener('scroll', handleScroll);
    };
  });
  
  function scrollToSection(id) {
    const el = document.getElementById(id);
    if (el) {
      el.scrollIntoView({ behavior: 'smooth' });
    }
    mobileMenuOpen = false;
  }
  
  function toggleCard(id) {
    expandedCard = expandedCard === id ? null : id;
  }
  
  function handleSubmit(e) {
    e.preventDefault();
    formStatus = 'sending';
    
    // Simulate form submission
    setTimeout(() => {
      formStatus = 'success';
      formData = { name: '', email: '', message: '' };
      setTimeout(() => {
        formStatus = 'idle';
      }, 3000);
    }, 1500);
  }
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
</svelte:head>

<div class="app">
  <!-- Navigation -->
  <nav class="nav" class:scrolled={scrollY > 50}>
    <div class="nav-container">
      <a href="#home" class="nav-logo" onclick={(e) => { e.preventDefault(); scrollToSection('home'); }}>
        <span class="logo-text">GT</span>
        <span class="logo-dot"></span>
      </a>
      
      <div class="nav-links">
        {#each navItems as item}
          <a 
            href="#{item.id}" 
            class="nav-link"
            class:active={activeSection === item.id}
            onclick={(e) => { e.preventDefault(); scrollToSection(item.id); }}
          >
            {item.label}
          </a>
        {/each}
      </div>
      
      <!-- Language Switcher -->
      <div class="language-switcher">
        <button 
          class="language-btn"
          onclick={() => languageDropdownOpen = !languageDropdownOpen}
        >
          <span class="globe-icon">🌐</span>
          <span>EN</span>
          <span class="chevron" class:open={languageDropdownOpen}>▼</span>
        </button>
        
        {#if languageDropdownOpen}
          <div class="language-dropdown">
            {#each languages as lang}
              <button class="language-option">
                <span>{lang.flag}</span>
                <span>{lang.name}</span>
                <span class="level-badge">{lang.level}</span>
              </button>
            {/each}
          </div>
        {/if}
      </div>
      
      <!-- Mobile Menu Button -->
      <button class="mobile-menu-btn" onclick={() => mobileMenuOpen = !mobileMenuOpen}>
        <span class="hamburger" class:open={mobileMenuOpen}></span>
      </button>
    </div>
    
    <!-- Mobile Menu -->
    {#if mobileMenuOpen}
      <div class="mobile-menu">
        {#each navItems as item}
          <a 
            href="#{item.id}"
            class="mobile-link"
            onclick={(e) => { e.preventDefault(); scrollToSection(item.id); }}
          >
            {item.label}
          </a>
        {/each}
      </div>
    {/if}
  </nav>

  <!-- Hero Section -->
  <section id="home" class="hero">
    <div class="hero-bg">
      <div class="orb orb-1"></div>
      <div class="orb orb-2"></div>
    </div>
    
    <div class="hero-content" class:visible={mounted}>
      <div class="profile-container">
        <div class="profile-ring">
          <div class="profile-image">
            <span class="profile-initials">GT</span>
          </div>
        </div>
        <div class="status-indicator">
          <span class="status-dot"></span>
          <span>Available for opportunities</span>
        </div>
      </div>
      
      <h1 class="hero-title">Giorgi Tchanturia</h1>
      <p class="hero-subtitle">
        <span class="typed-text">{typedText}</span>
        <span class="cursor">|</span>
      </p>
      
      <p class="hero-description">
        Building resilient, scalable systems that power modern enterprises. 
        Passionate about infrastructure automation, cloud-native architecture, 
        and DevOps best practices.
      </p>
      
      <div class="hero-cta">
        <button class="btn-primary" onclick={() => scrollToSection('contact')}>
          Get in Touch
          <span class="btn-arrow">→</span>
        </button>
        <button class="btn-secondary" onclick={() => scrollToSection('systems')}>
          View My Work
        </button>
      </div>
      
      <div class="hero-stats">
        <div class="stat">
          <span class="stat-value">5+</span>
          <span class="stat-label">Years Experience</span>
        </div>
        <div class="stat">
          <span class="stat-value">50+</span>
          <span class="stat-label">Projects Delivered</span>
        </div>
        <div class="stat">
          <span class="stat-value">99.9%</span>
          <span class="stat-label">Uptime Achieved</span>
        </div>
      </div>
    </div>
    
    <div class="scroll-indicator" onclick={() => scrollToSection('systems')}>
      <span>Scroll to explore</span>
      <div class="scroll-arrow"></div>
    </div>
  </section>

  <!-- Systems Architecture Section -->
  <section id="systems" class="systems">
    <div class="section-container">
      <div class="section-header">
        <span class="section-tag">Engineering</span>
        <h2 class="section-title">Systems Architecture</h2>
        <p class="section-description">
          Crafting robust infrastructure solutions with a focus on scalability, 
          security, and operational excellence.
        </p>
      </div>
      
      <div class="systems-grid">
        {#each systemsCards as card}
          <div 
            class="system-card"
            class:expanded={expandedCard === card.id}
          >
            <div class="card-header" onclick={() => toggleCard(card.id)}>
              <div class="card-icon">
                {#if card.id === 'infrastructure'}
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M2 20h20M5 20V8l7-5 7 5v12"/>
                    <path d="M9 20v-4h6v4"/>
                  </svg>
                {:else if card.id === 'devops'}
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="12" r="3"/>
                    <path d="M12 1v4M12 19v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M1 12h4M19 12h4M4.93 19.07l2.83-2.83M16.24 7.76l2.83-2.83"/>
                  </svg>
                {:else}
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>
                  </svg>
                {/if}
              </div>
              <div class="card-titles">
                <h3>{card.title}</h3>
                <span class="card-subtitle">{card.subtitle}</span>
              </div>
              <span class="expand-icon" class:rotated={expandedCard === card.id}>+</span>
            </div>
            
            {#if expandedCard === card.id}
              <div class="card-content">
                <p>{card.description}</p>
                
                <div class="tech-stack">
                  {#each card.technologies as tech}
                    <span class="tech-tag">{tech}</span>
                  {/each}
                </div>
                
                <div class="code-block">
                  <div class="code-header">
                    <span class="code-dot red"></span>
                    <span class="code-dot yellow"></span>
                    <span class="code-dot green"></span>
                  </div>
                  <pre><code>{card.codeSnippet}</code></pre>
                </div>
              </div>
            {/if}
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- Resume Section -->
  <section id="resume" class="resume">
    <div class="section-container">
      <div class="section-header">
        <span class="section-tag">Background</span>
        <h2 class="section-title">Skills & Experience</h2>
        <p class="section-description">
          A comprehensive overview of my technical expertise and professional journey.
        </p>
      </div>
      
      <div class="resume-content">
        <div class="skills-grid">
          {#each skills as skill}
            <div class="skill-card">
              <h4>{skill.name}</h4>
              <div class="skill-items">
                {#each skill.items as item}
                  <span class="skill-item">{item}</span>
                {/each}
              </div>
            </div>
          {/each}
        </div>
        
        <div class="cv-preview">
          <div class="cv-card">
            <div class="cv-header">
              <span class="cv-icon">📄</span>
              <div>
                <h4>Download Resume</h4>
                <p>PDF format, last updated 2025</p>
              </div>
            </div>
            <button class="download-btn">
              <span>Download CV</span>
              <span class="download-icon">↓</span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Contact Section -->
  <section id="contact" class="contact">
    <div class="section-container">
      <div class="section-header">
        <span class="section-tag">Connect</span>
        <h2 class="section-title">Get in Touch</h2>
        <p class="section-description">
          Have a project in mind or want to discuss opportunities? 
          I'd love to hear from you.
        </p>
      </div>
      
      <div class="contact-content">
        <div class="terminal">
          <div class="terminal-header">
            <span class="terminal-dot red"></span>
            <span class="terminal-dot yellow"></span>
            <span class="terminal-dot green"></span>
            <span class="terminal-title">contact@giorgi ~ zsh</span>
          </div>
          <div class="terminal-body">
            <form onsubmit={handleSubmit}>
              <div class="terminal-line">
                <span class="prompt">$</span>
                <span class="command">whoami</span>
              </div>
              <div class="input-group">
                <label for="name">name:</label>
                <input 
                  type="text" 
                  id="name" 
                  bind:value={formData.name}
                  placeholder="Your name"
                  required
                />
              </div>
              
              <div class="terminal-line">
                <span class="prompt">$</span>
                <span class="command">mail -s "Contact"</span>
              </div>
              <div class="input-group">
                <label for="email">from:</label>
                <input 
                  type="email" 
                  id="email" 
                  bind:value={formData.email}
                  placeholder="your@email.com"
                  required
                />
              </div>
              
              <div class="terminal-line">
                <span class="prompt">$</span>
                <span class="command">cat {'>'} message.txt</span>
              </div>
              <div class="input-group">
                <textarea 
                  id="message" 
                  bind:value={formData.message}
                  placeholder="Your message..."
                  rows="4"
                  required
                ></textarea>
              </div>
              
              <div class="terminal-line">
                <span class="prompt">$</span>
                <button type="submit" class="submit-btn" disabled={formStatus === 'sending'}>
                  {#if formStatus === 'idle'}
                    ./send_message.sh
                  {:else if formStatus === 'sending'}
                    sending...
                  {:else}
                    Message sent!
                  {/if}
                </button>
              </div>
            </form>
          </div>
        </div>
        
        <div class="contact-info">
          <div class="info-card">
            <span class="info-icon">📧</span>
            <div>
              <h4>Email</h4>
              <a href="mailto:hello@giorgitchanturia.com">hello@giorgitchanturia.com</a>
            </div>
          </div>
          <div class="info-card">
            <span class="info-icon">📍</span>
            <div>
              <h4>Location</h4>
              <p>Tbilisi, Georgia</p>
            </div>
          </div>
          <div class="info-card">
            <span class="info-icon">💼</span>
            <div>
              <h4>LinkedIn</h4>
              <a href="#">/in/giorgitchanturia</a>
            </div>
          </div>
          <div class="info-card">
            <span class="info-icon">🐙</span>
            <div>
              <h4>GitHub</h4>
              <a href="#">@gchantura</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="footer">
    <div class="footer-container">
      <p>&copy; 2025 Giorgi Tchanturia. All rights reserved.</p>
    </div>
  </footer>
</div>

<style>
  /* Reset & Base */
  :global(*), :global(*::before), :global(*::after) {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  :global(html) {
    scroll-behavior: smooth;
  }

  :global(body) {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    background-color: #020617;
    color: #e2e8f0;
    line-height: 1.6;
    overflow-x: hidden;
  }

  .app {
    min-height: 100vh;
    background-image: 
      linear-gradient(rgba(34, 211, 238, 0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(34, 211, 238, 0.03) 1px, transparent 1px);
    background-size: 50px 50px;
  }

  /* Navigation */
  .nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;
    padding: 1rem 2rem;
    transition: all 0.3s ease;
  }

  .nav.scrolled {
    background: rgba(2, 6, 23, 0.9);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid rgba(34, 211, 238, 0.1);
  }

  .nav-container {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .nav-logo {
    display: flex;
    align-items: center;
    gap: 0.25rem;
    font-size: 1.5rem;
    font-weight: 700;
    color: #e2e8f0;
    text-decoration: none;
  }

  .logo-dot {
    width: 8px;
    height: 8px;
    background: #22d3ee;
    border-radius: 50%;
  }

  .nav-links {
    display: flex;
    gap: 2rem;
  }

  .nav-link {
    color: #94a3b8;
    text-decoration: none;
    font-size: 0.875rem;
    font-weight: 500;
    transition: color 0.2s;
    position: relative;
  }

  .nav-link:hover, .nav-link.active {
    color: #22d3ee;
  }

  .nav-link.active::after {
    content: '';
    position: absolute;
    bottom: -4px;
    left: 0;
    right: 0;
    height: 2px;
    background: #22d3ee;
    border-radius: 1px;
  }

  /* Language Switcher */
  .language-switcher {
    position: relative;
  }

  .language-btn {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.5rem 1rem;
    background: rgba(51, 65, 85, 0.5);
    border: 1px solid rgba(71, 85, 105, 0.5);
    border-radius: 8px;
    color: #e2e8f0;
    font-size: 0.875rem;
    cursor: pointer;
    transition: all 0.2s;
  }

  .language-btn:hover {
    background: rgba(51, 65, 85, 0.8);
    border-color: #22d3ee;
  }

  .chevron {
    font-size: 0.625rem;
    transition: transform 0.2s;
  }

  .chevron.open {
    transform: rotate(180deg);
  }

  .language-dropdown {
    position: absolute;
    top: calc(100% + 0.5rem);
    right: 0;
    background: rgba(15, 23, 42, 0.95);
    backdrop-filter: blur(12px);
    border: 1px solid rgba(71, 85, 105, 0.5);
    border-radius: 12px;
    padding: 0.5rem;
    min-width: 180px;
  }

  .language-option {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    width: 100%;
    padding: 0.75rem 1rem;
    background: transparent;
    border: none;
    color: #e2e8f0;
    font-size: 0.875rem;
    cursor: pointer;
    border-radius: 8px;
    transition: background 0.2s;
  }

  .language-option:hover {
    background: rgba(34, 211, 238, 0.1);
  }

  .level-badge {
    margin-left: auto;
    font-size: 0.75rem;
    color: #22d3ee;
    background: rgba(34, 211, 238, 0.1);
    padding: 0.125rem 0.5rem;
    border-radius: 4px;
  }

  /* Mobile Menu */
  .mobile-menu-btn {
    display: none;
    padding: 0.5rem;
    background: transparent;
    border: none;
    cursor: pointer;
  }

  .hamburger {
    display: block;
    width: 24px;
    height: 2px;
    background: #e2e8f0;
    position: relative;
    transition: background 0.2s;
  }

  .hamburger::before, .hamburger::after {
    content: '';
    position: absolute;
    left: 0;
    width: 100%;
    height: 2px;
    background: #e2e8f0;
    transition: transform 0.2s;
  }

  .hamburger::before { top: -8px; }
  .hamburger::after { bottom: -8px; }

  .hamburger.open {
    background: transparent;
  }

  .hamburger.open::before {
    transform: rotate(45deg) translate(5px, 6px);
  }

  .hamburger.open::after {
    transform: rotate(-45deg) translate(5px, -6px);
  }

  .mobile-menu {
    display: none;
    flex-direction: column;
    gap: 1rem;
    padding: 1rem;
    background: rgba(15, 23, 42, 0.98);
    border-top: 1px solid rgba(71, 85, 105, 0.3);
  }

  .mobile-link {
    color: #e2e8f0;
    text-decoration: none;
    font-size: 1.125rem;
    padding: 0.75rem;
  }

  /* Hero Section */
  .hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 6rem 2rem 2rem;
    position: relative;
    overflow: hidden;
  }

  .hero-bg {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }

  .orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0.5;
  }

  .orb-1 {
    width: 400px;
    height: 400px;
    background: rgba(34, 211, 238, 0.2);
    top: 10%;
    left: 10%;
    animation: float 8s ease-in-out infinite;
  }

  .orb-2 {
    width: 300px;
    height: 300px;
    background: rgba(6, 182, 212, 0.15);
    bottom: 20%;
    right: 10%;
    animation: float 10s ease-in-out infinite reverse;
  }

  @keyframes float {
    0%, 100% { transform: translate(0, 0); }
    50% { transform: translate(30px, -30px); }
  }

  .hero-content {
    text-align: center;
    max-width: 800px;
    z-index: 1;
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.8s ease;
  }

  .hero-content.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .profile-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
    margin-bottom: 2rem;
  }

  .profile-ring {
    width: 140px;
    height: 140px;
    border-radius: 50%;
    padding: 4px;
    background: linear-gradient(135deg, #22d3ee, #06b6d4, #0891b2);
    animation: pulse-ring 2s ease-in-out infinite;
  }

  @keyframes pulse-ring {
    0%, 100% { box-shadow: 0 0 0 0 rgba(34, 211, 238, 0.4); }
    50% { box-shadow: 0 0 0 15px rgba(34, 211, 238, 0); }
  }

  .profile-image {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: linear-gradient(135deg, #0f172a, #1e293b);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .profile-initials {
    font-size: 2.5rem;
    font-weight: 700;
    color: #22d3ee;
  }

  .status-indicator {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.5rem 1rem;
    background: rgba(34, 197, 94, 0.1);
    border: 1px solid rgba(34, 197, 94, 0.3);
    border-radius: 9999px;
    font-size: 0.875rem;
    color: #4ade80;
  }

  .status-dot {
    width: 8px;
    height: 8px;
    background: #4ade80;
    border-radius: 50%;
    animation: blink 2s ease-in-out infinite;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  .hero-title {
    font-size: clamp(2.5rem, 6vw, 4rem);
    font-weight: 700;
    color: #f8fafc;
    margin-bottom: 0.5rem;
    letter-spacing: -0.02em;
  }

  .hero-subtitle {
    font-size: 1.25rem;
    color: #22d3ee;
    margin-bottom: 1.5rem;
    font-family: 'JetBrains Mono', monospace;
  }

  .cursor {
    animation: blink-cursor 1s step-end infinite;
  }

  @keyframes blink-cursor {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  .hero-description {
    font-size: 1.125rem;
    color: #94a3b8;
    max-width: 600px;
    margin: 0 auto 2rem;
    line-height: 1.7;
  }

  .hero-cta {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 3rem;
  }

  .btn-primary, .btn-secondary {
    padding: 0.875rem 2rem;
    font-size: 1rem;
    font-weight: 500;
    border-radius: 12px;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .btn-primary {
    background: linear-gradient(135deg, #22d3ee, #06b6d4);
    color: #020617;
    border: none;
  }

  .btn-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 30px rgba(34, 211, 238, 0.3);
  }

  .btn-arrow {
    transition: transform 0.2s;
  }

  .btn-primary:hover .btn-arrow {
    transform: translateX(4px);
  }

  .btn-secondary {
    background: transparent;
    color: #e2e8f0;
    border: 1px solid rgba(71, 85, 105, 0.5);
  }

  .btn-secondary:hover {
    border-color: #22d3ee;
    color: #22d3ee;
  }

  .hero-stats {
    display: flex;
    justify-content: center;
    gap: 3rem;
    flex-wrap: wrap;
  }

  .stat {
    text-align: center;
  }

  .stat-value {
    display: block;
    font-size: 2rem;
    font-weight: 700;
    color: #f8fafc;
  }

  .stat-label {
    font-size: 0.875rem;
    color: #64748b;
  }

  .scroll-indicator {
    position: absolute;
    bottom: 2rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
    color: #64748b;
    font-size: 0.875rem;
    cursor: pointer;
    animation: bounce 2s ease-in-out infinite;
  }

  .scroll-arrow {
    width: 20px;
    height: 20px;
    border-right: 2px solid #64748b;
    border-bottom: 2px solid #64748b;
    transform: rotate(45deg);
  }

  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(8px); }
  }

  /* Section Styles */
  .section-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 6rem 2rem;
  }

  .section-header {
    text-align: center;
    margin-bottom: 4rem;
  }

  .section-tag {
    display: inline-block;
    padding: 0.375rem 1rem;
    background: rgba(34, 211, 238, 0.1);
    color: #22d3ee;
    font-size: 0.75rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    border-radius: 9999px;
    margin-bottom: 1rem;
  }

  .section-title {
    font-size: clamp(2rem, 4vw, 3rem);
    font-weight: 700;
    color: #f8fafc;
    margin-bottom: 1rem;
  }

  .section-description {
    font-size: 1.125rem;
    color: #94a3b8;
    max-width: 600px;
    margin: 0 auto;
  }

  /* Systems Section */
  .systems {
    background: linear-gradient(180deg, transparent, rgba(15, 23, 42, 0.5), transparent);
  }

  .systems-grid {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .system-card {
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(71, 85, 105, 0.3);
    border-radius: 16px;
    overflow: hidden;
    transition: all 0.3s ease;
  }

  .system-card:hover {
    border-color: rgba(34, 211, 238, 0.3);
  }

  .system-card.expanded {
    border-color: #22d3ee;
    background: rgba(15, 23, 42, 0.8);
  }

  .card-header {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1.5rem;
    cursor: pointer;
  }

  .card-icon {
    width: 48px;
    height: 48px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(34, 211, 238, 0.1);
    border-radius: 12px;
    color: #22d3ee;
  }

  .card-icon svg {
    width: 24px;
    height: 24px;
  }

  .card-titles {
    flex: 1;
  }

  .card-titles h3 {
    font-size: 1.125rem;
    font-weight: 600;
    color: #f8fafc;
  }

  .card-subtitle {
    font-size: 0.875rem;
    color: #64748b;
  }

  .expand-icon {
    font-size: 1.5rem;
    color: #64748b;
    transition: transform 0.3s;
  }

  .expand-icon.rotated {
    transform: rotate(45deg);
  }

  .card-content {
    padding: 0 1.5rem 1.5rem;
    border-top: 1px solid rgba(71, 85, 105, 0.3);
  }

  .card-content p {
    color: #94a3b8;
    margin: 1rem 0;
  }

  .tech-stack {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 1rem;
  }

  .tech-tag {
    padding: 0.25rem 0.75rem;
    background: rgba(34, 211, 238, 0.1);
    color: #22d3ee;
    font-size: 0.75rem;
    border-radius: 6px;
  }

  .code-block {
    background: #0f172a;
    border-radius: 12px;
    overflow: hidden;
    font-family: 'JetBrains Mono', monospace;
  }

  .code-header {
    display: flex;
    gap: 0.5rem;
    padding: 0.75rem 1rem;
    background: rgba(30, 41, 59, 0.5);
  }

  .code-dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
  }

  .code-dot.red { background: #ef4444; }
  .code-dot.yellow { background: #eab308; }
  .code-dot.green { background: #22c55e; }

  .code-block pre {
    padding: 1rem;
    overflow-x: auto;
    margin: 0;
  }

  .code-block code {
    font-size: 0.8rem;
    color: #a5f3fc;
  }

  /* Resume Section */
  .resume-content {
    display: grid;
    grid-template-columns: 1fr 300px;
    gap: 3rem;
    align-items: start;
  }

  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
  }

  .skill-card {
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(71, 85, 105, 0.3);
    border-radius: 12px;
    padding: 1.5rem;
  }

  .skill-card h4 {
    font-size: 0.875rem;
    color: #64748b;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 1rem;
  }

  .skill-items {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .skill-item {
    padding: 0.375rem 0.75rem;
    background: rgba(34, 211, 238, 0.1);
    color: #e2e8f0;
    font-size: 0.875rem;
    border-radius: 6px;
  }

  .cv-preview {
    position: sticky;
    top: 6rem;
  }

  .cv-card {
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(71, 85, 105, 0.3);
    border-radius: 16px;
    padding: 1.5rem;
  }

  .cv-header {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .cv-icon {
    font-size: 2rem;
  }

  .cv-header h4 {
    font-size: 1rem;
    color: #f8fafc;
  }

  .cv-header p {
    font-size: 0.875rem;
    color: #64748b;
  }

  .download-btn {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    padding: 0.875rem;
    background: linear-gradient(135deg, #22d3ee, #06b6d4);
    color: #020617;
    font-weight: 600;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .download-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(34, 211, 238, 0.3);
  }

  /* Contact Section */
  .contact {
    background: linear-gradient(180deg, transparent, rgba(15, 23, 42, 0.5));
  }

  .contact-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 3rem;
    align-items: start;
  }

  .terminal {
    background: #0f172a;
    border: 1px solid rgba(71, 85, 105, 0.3);
    border-radius: 16px;
    overflow: hidden;
  }

  .terminal-header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.75rem 1rem;
    background: rgba(30, 41, 59, 0.5);
    border-bottom: 1px solid rgba(71, 85, 105, 0.3);
  }

  .terminal-dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
  }

  .terminal-dot.red { background: #ef4444; }
  .terminal-dot.yellow { background: #eab308; }
  .terminal-dot.green { background: #22c55e; }

  .terminal-title {
    margin-left: auto;
    font-size: 0.75rem;
    color: #64748b;
    font-family: 'JetBrains Mono', monospace;
  }

  .terminal-body {
    padding: 1.5rem;
    font-family: 'JetBrains Mono', monospace;
  }

  .terminal-line {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin-bottom: 0.5rem;
  }

  .prompt {
    color: #22d3ee;
  }

  .command {
    color: #4ade80;
  }

  .input-group {
    margin-bottom: 1.5rem;
  }

  .input-group label {
    display: block;
    color: #64748b;
    font-size: 0.75rem;
    margin-bottom: 0.5rem;
  }

  .input-group input, .input-group textarea {
    width: 100%;
    padding: 0.75rem;
    background: rgba(30, 41, 59, 0.5);
    border: 1px solid rgba(71, 85, 105, 0.5);
    border-radius: 8px;
    color: #e2e8f0;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.875rem;
    outline: none;
    transition: border-color 0.2s;
  }

  .input-group input:focus, .input-group textarea:focus {
    border-color: #22d3ee;
  }

  .input-group textarea {
    resize: vertical;
    min-height: 100px;
  }

  .submit-btn {
    background: transparent;
    border: none;
    color: #22d3ee;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.875rem;
    cursor: pointer;
    transition: color 0.2s;
  }

  .submit-btn:hover:not(:disabled) {
    color: #67e8f9;
  }

  .submit-btn:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }

  .contact-info {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .info-card {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1.25rem;
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(71, 85, 105, 0.3);
    border-radius: 12px;
    transition: all 0.2s;
  }

  .info-card:hover {
    border-color: rgba(34, 211, 238, 0.3);
  }

  .info-icon {
    font-size: 1.5rem;
  }

  .info-card h4 {
    font-size: 0.875rem;
    color: #f8fafc;
    margin-bottom: 0.25rem;
  }

  .info-card p, .info-card a {
    font-size: 0.875rem;
    color: #64748b;
  }

  .info-card a {
    text-decoration: none;
    transition: color 0.2s;
  }

  .info-card a:hover {
    color: #22d3ee;
  }

  /* Footer */
  .footer {
    border-top: 1px solid rgba(71, 85, 105, 0.3);
    padding: 2rem;
    text-align: center;
  }

  .footer p {
    color: #64748b;
    font-size: 0.875rem;
  }

  /* Responsive */
  @media (max-width: 1024px) {
    .resume-content {
      grid-template-columns: 1fr;
    }

    .cv-preview {
      position: static;
    }

    .contact-content {
      grid-template-columns: 1fr;
    }
  }

  @media (max-width: 768px) {
    .nav-links {
      display: none;
    }

    .language-switcher {
      display: none;
    }

    .mobile-menu-btn {
      display: block;
    }

    .mobile-menu {
      display: flex;
    }

    .hero-stats {
      gap: 1.5rem;
    }

    .stat-value {
      font-size: 1.5rem;
    }
  }
</style>
