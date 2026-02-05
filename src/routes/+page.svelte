<script>
  import { onMount } from 'svelte';

  let activeSection = $state('home');
  let mobileMenuOpen = $state(false);
  let scrollY = $state(0);
  let typedText = $state('');
  let expandedCard = $state(null);
  let langDropdownOpen = $state(false);

  const fullText = 'Software Engineer & Systems Architect';
  const sections = ['home', 'systems', 'resume', 'contact'];
  const languages = [
    { code: 'ka', name: 'Georgian', level: 'Native' },
    { code: 'en', name: 'English', level: 'C1' },
    { code: 'ru', name: 'Russian', level: 'C1' }
  ];

  const systemCards = [
    {
      id: 'architecture',
      title: 'System Design',
      description: 'Building scalable distributed systems with clean architecture principles',
      code: `interface SystemLayer {
  presentation: UIComponent[];
  application: UseCase[];
  domain: Entity[];
  infrastructure: Repository[];
}`
    },
    {
      id: 'backend',
      title: 'Backend Engineering',
      description: 'RESTful APIs, microservices, and event-driven architectures',
      code: `async function processRequest(req) {
  const validated = await validate(req);
  const result = await useCase.execute(validated);
  return Response.json(result);
}`
    },
    {
      id: 'devops',
      title: 'DevOps & Cloud',
      description: 'CI/CD pipelines, containerization, and cloud infrastructure',
      code: `pipeline:
  build:
    - npm ci && npm run build
  deploy:
    - docker push $IMAGE
    - kubectl apply -f k8s/`
    }
  ];

  const skills = [
    { category: 'Languages', items: ['TypeScript', 'Python', 'Go', 'Java'] },
    { category: 'Frontend', items: ['React', 'Svelte', 'Next.js', 'Vue'] },
    { category: 'Backend', items: ['Node.js', 'FastAPI', 'Spring', 'GraphQL'] },
    { category: 'Cloud', items: ['AWS', 'GCP', 'Docker', 'Kubernetes'] }
  ];

  onMount(() => {
    let i = 0;
    const typeInterval = setInterval(() => {
      if (i < fullText.length) {
        typedText = fullText.slice(0, i + 1);
        i++;
      } else {
        clearInterval(typeInterval);
      }
    }, 50);

    const handleScroll = () => {
      scrollY = window.scrollY;
      for (const section of sections) {
        const el = document.getElementById(section);
        if (el) {
          const rect = el.getBoundingClientRect();
          if (rect.top <= 150 && rect.bottom > 150) {
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

  function scrollTo(id) {
    document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
    mobileMenuOpen = false;
  }

  function toggleCard(id) {
    expandedCard = expandedCard === id ? null : id;
  }
</script>

<svelte:head>
  <title>Giorgi Tchanturia | Software Engineer</title>
  <meta name="description" content="Portfolio of Giorgi Tchanturia - Software Engineer & Systems Architect" />
</svelte:head>

<!-- Navigation -->
<nav class="nav" class:scrolled={scrollY > 50}>
  <div class="nav-container">
    <a href="#home" class="logo" onclick={(e) => { e.preventDefault(); scrollTo('home'); }}>
      <span class="logo-bracket">&lt;</span>GT<span class="logo-bracket">/&gt;</span>
    </a>

    <div class="nav-links">
      {#each sections as section}
        <button
          class="nav-link"
          class:active={activeSection === section}
          onclick={() => scrollTo(section)}
        >
          {section.charAt(0).toUpperCase() + section.slice(1)}
        </button>
      {/each}
    </div>

    <button class="mobile-toggle" onclick={() => mobileMenuOpen = !mobileMenuOpen}>
      <span class="hamburger" class:open={mobileMenuOpen}></span>
    </button>
  </div>
</nav>

<!-- Mobile Menu -->
{#if mobileMenuOpen}
  <div class="mobile-menu">
    {#each sections as section}
      <button class="mobile-link" onclick={() => scrollTo(section)}>
        {section.charAt(0).toUpperCase() + section.slice(1)}
      </button>
    {/each}
  </div>
{/if}

<!-- Hero Section -->
<section id="home" class="hero">
  <div class="hero-bg"></div>
  <div class="hero-content">
    <div class="profile-ring">
      <div class="profile-inner">
        <span class="profile-initials">GT</span>
      </div>
    </div>
    
    <h1 class="hero-name">Giorgi Tchanturia</h1>
    <p class="hero-role">{typedText}<span class="cursor">|</span></p>
    
    <!-- Language Switcher -->
    <div class="lang-switcher">
      <button class="lang-toggle" onclick={() => langDropdownOpen = !langDropdownOpen}>
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="10"/>
          <path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
        </svg>
        <span>Languages</span>
        <svg class="chevron" class:open={langDropdownOpen} width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
      {#if langDropdownOpen}
        <div class="lang-dropdown">
          {#each languages as lang}
            <div class="lang-item">
              <span class="lang-name">{lang.name}</span>
              <span class="lang-level">{lang.level}</span>
            </div>
          {/each}
        </div>
      {/if}
    </div>

    <div class="hero-cta">
      <button class="btn-primary" onclick={() => scrollTo('contact')}>
        Get in Touch
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M5 12h14M12 5l7 7-7 7"/>
        </svg>
      </button>
      <button class="btn-secondary" onclick={() => scrollTo('systems')}>
        View Work
      </button>
    </div>
  </div>
</section>

<!-- Systems Architecture Section -->
<section id="systems" class="section">
  <div class="container">
    <h2 class="section-title">
      <span class="title-decoration">//</span> Systems Architecture
    </h2>
    <p class="section-subtitle">Building robust, scalable solutions with clean code principles</p>
    
    <div class="cards-grid">
      {#each systemCards as card}
        <button class="system-card" class:expanded={expandedCard === card.id} onclick={() => toggleCard(card.id)}>
          <div class="card-header">
            <h3 class="card-title">{card.title}</h3>
            <svg class="card-icon" class:rotated={expandedCard === card.id} width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="m6 9 6 6 6-6"/>
            </svg>
          </div>
          <p class="card-desc">{card.description}</p>
          
          {#if expandedCard === card.id}
            <div class="code-block">
              <div class="code-header">
                <span class="code-dot red"></span>
                <span class="code-dot yellow"></span>
                <span class="code-dot green"></span>
              </div>
              <pre class="code-content">{card.code}</pre>
            </div>
          {/if}
        </button>
      {/each}
    </div>
  </div>
</section>

<!-- Resume Section -->
<section id="resume" class="section section-alt">
  <div class="container">
    <h2 class="section-title">
      <span class="title-decoration">//</span> Skills & Resume
    </h2>
    
    <div class="skills-grid">
      {#each skills as skillGroup}
        <div class="skill-card">
          <h3 class="skill-category">{skillGroup.category}</h3>
          <div class="skill-items">
            {#each skillGroup.items as item}
              <span class="skill-tag">{item}</span>
            {/each}
          </div>
        </div>
      {/each}
    </div>
    
    <div class="cv-section">
      <div class="cv-preview">
        <div class="cv-line"></div>
        <div class="cv-line short"></div>
        <div class="cv-line"></div>
        <div class="cv-line medium"></div>
      </div>
      <button class="download-btn">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4M7 10l5 5 5-5M12 15V3"/>
        </svg>
        Download CV
      </button>
    </div>
  </div>
</section>

<!-- Contact Section -->
<section id="contact" class="section">
  <div class="container">
    <h2 class="section-title">
      <span class="title-decoration">//</span> Contact
    </h2>
    
    <div class="terminal">
      <div class="terminal-header">
        <span class="terminal-dot red"></span>
        <span class="terminal-dot yellow"></span>
        <span class="terminal-dot green"></span>
        <span class="terminal-title">zsh - contact</span>
      </div>
      <div class="terminal-body">
        <div class="terminal-line">
          <span class="prompt">~</span>
          <span class="command">./send-message.sh</span>
        </div>
        
        <form class="contact-form">
          <div class="form-group">
            <label class="form-label">
              <span class="prompt">name:</span>
            </label>
            <input type="text" class="form-input" placeholder="Your name" />
          </div>
          
          <div class="form-group">
            <label class="form-label">
              <span class="prompt">email:</span>
            </label>
            <input type="email" class="form-input" placeholder="your@email.com" />
          </div>
          
          <div class="form-group">
            <label class="form-label">
              <span class="prompt">message:</span>
            </label>
            <textarea class="form-textarea" placeholder="Your message..." rows="4"></textarea>
          </div>
          
          <button type="submit" class="submit-btn">
            <span class="prompt">$</span> submit --send
          </button>
        </form>
      </div>
    </div>
    
    <div class="social-links">
      <a href="https://github.com" target="_blank" rel="noopener" class="social-link">GitHub</a>
      <a href="https://linkedin.com" target="_blank" rel="noopener" class="social-link">LinkedIn</a>
      <a href="mailto:contact@example.com" class="social-link">Email</a>
    </div>
  </div>
</section>

<footer class="footer">
  <p>&copy; 2026 Giorgi Tchanturia. All rights reserved.</p>
</footer>

<style>
  /* Navigation */
  .nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;
    padding: 1rem 2rem;
    transition: background-color 0.3s, backdrop-filter 0.3s;
  }

  .nav.scrolled {
    background-color: rgba(2, 6, 23, 0.9);
    backdrop-filter: blur(10px);
  }

  .nav-container {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo {
    font-family: 'JetBrains Mono', monospace;
    font-size: 1.5rem;
    font-weight: 700;
    color: #22d3ee;
  }

  .logo-bracket {
    color: #64748b;
  }

  .nav-links {
    display: flex;
    gap: 2rem;
  }

  .nav-link {
    color: #94a3b8;
    font-size: 0.9rem;
    transition: color 0.2s;
    padding: 0.5rem;
  }

  .nav-link:hover, .nav-link.active {
    color: #22d3ee;
  }

  .mobile-toggle {
    display: none;
    padding: 0.5rem;
  }

  .hamburger {
    display: block;
    width: 24px;
    height: 2px;
    background: #e2e8f0;
    position: relative;
  }

  .hamburger::before, .hamburger::after {
    content: '';
    position: absolute;
    width: 24px;
    height: 2px;
    background: #e2e8f0;
    transition: transform 0.3s;
  }

  .hamburger::before { top: -7px; }
  .hamburger::after { top: 7px; }

  .hamburger.open {
    background: transparent;
  }

  .hamburger.open::before {
    transform: translateY(7px) rotate(45deg);
  }

  .hamburger.open::after {
    transform: translateY(-7px) rotate(-45deg);
  }

  .mobile-menu {
    position: fixed;
    top: 60px;
    left: 0;
    right: 0;
    background: rgba(2, 6, 23, 0.98);
    padding: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    z-index: 99;
  }

  .mobile-link {
    color: #e2e8f0;
    font-size: 1.1rem;
    padding: 1rem;
    text-align: left;
  }

  /* Hero */
  .hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    padding: 2rem;
  }

  .hero-bg {
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at 50% 0%, rgba(34, 211, 238, 0.15) 0%, transparent 50%);
    pointer-events: none;
  }

  .hero-content {
    text-align: center;
    position: relative;
    z-index: 1;
  }

  .profile-ring {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    background: linear-gradient(135deg, #22d3ee, #0891b2);
    padding: 4px;
    margin: 0 auto 2rem;
    animation: pulse 3s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { box-shadow: 0 0 0 0 rgba(34, 211, 238, 0.4); }
    50% { box-shadow: 0 0 0 20px rgba(34, 211, 238, 0); }
  }

  .profile-inner {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: #0f172a;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .profile-initials {
    font-family: 'JetBrains Mono', monospace;
    font-size: 3rem;
    font-weight: 700;
    color: #22d3ee;
  }

  .hero-name {
    font-size: clamp(2rem, 5vw, 3.5rem);
    font-weight: 700;
    margin-bottom: 0.5rem;
    color: #f8fafc;
  }

  .hero-role {
    font-family: 'JetBrains Mono', monospace;
    font-size: 1.1rem;
    color: #22d3ee;
    margin-bottom: 2rem;
  }

  .cursor {
    animation: blink 1s step-end infinite;
  }

  @keyframes blink {
    50% { opacity: 0; }
  }

  /* Language Switcher */
  .lang-switcher {
    position: relative;
    display: inline-block;
    margin-bottom: 2rem;
  }

  .lang-toggle {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.75rem 1.25rem;
    background: rgba(34, 211, 238, 0.1);
    border: 1px solid rgba(34, 211, 238, 0.3);
    border-radius: 9999px;
    color: #e2e8f0;
    font-size: 0.9rem;
    transition: all 0.2s;
  }

  .lang-toggle:hover {
    background: rgba(34, 211, 238, 0.2);
  }

  .chevron {
    transition: transform 0.2s;
  }

  .chevron.open {
    transform: rotate(180deg);
  }

  .lang-dropdown {
    position: absolute;
    top: calc(100% + 0.5rem);
    left: 50%;
    transform: translateX(-50%);
    background: rgba(15, 23, 42, 0.95);
    border: 1px solid rgba(34, 211, 238, 0.2);
    border-radius: 0.75rem;
    padding: 0.5rem;
    min-width: 180px;
    backdrop-filter: blur(10px);
  }

  .lang-item {
    display: flex;
    justify-content: space-between;
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;
  }

  .lang-item:hover {
    background: rgba(34, 211, 238, 0.1);
  }

  .lang-name {
    color: #e2e8f0;
  }

  .lang-level {
    color: #22d3ee;
    font-size: 0.85rem;
  }

  /* Buttons */
  .hero-cta {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
  }

  .btn-primary {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.875rem 1.75rem;
    background: linear-gradient(135deg, #22d3ee, #0891b2);
    color: #020617;
    font-weight: 600;
    border-radius: 9999px;
    transition: transform 0.2s, box-shadow 0.2s;
  }

  .btn-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 30px rgba(34, 211, 238, 0.3);
  }

  .btn-secondary {
    padding: 0.875rem 1.75rem;
    border: 1px solid rgba(34, 211, 238, 0.5);
    color: #22d3ee;
    border-radius: 9999px;
    transition: all 0.2s;
  }

  .btn-secondary:hover {
    background: rgba(34, 211, 238, 0.1);
  }

  /* Sections */
  .section {
    padding: 6rem 2rem;
  }

  .section-alt {
    background: rgba(15, 23, 42, 0.5);
  }

  .container {
    max-width: 1000px;
    margin: 0 auto;
  }

  .section-title {
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: 0.5rem;
    color: #f8fafc;
  }

  .title-decoration {
    color: #22d3ee;
    font-family: 'JetBrains Mono', monospace;
  }

  .section-subtitle {
    color: #64748b;
    margin-bottom: 3rem;
  }

  /* System Cards */
  .cards-grid {
    display: grid;
    gap: 1.5rem;
  }

  .system-card {
    text-align: left;
    padding: 1.5rem;
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(34, 211, 238, 0.1);
    border-radius: 1rem;
    transition: all 0.3s;
    width: 100%;
  }

  .system-card:hover {
    border-color: rgba(34, 211, 238, 0.3);
    background: rgba(15, 23, 42, 0.8);
  }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;
  }

  .card-title {
    font-size: 1.25rem;
    font-weight: 600;
    color: #f8fafc;
  }

  .card-icon {
    color: #22d3ee;
    transition: transform 0.3s;
  }

  .card-icon.rotated {
    transform: rotate(180deg);
  }

  .card-desc {
    color: #94a3b8;
    font-size: 0.95rem;
  }

  .code-block {
    margin-top: 1.5rem;
    border-radius: 0.75rem;
    overflow: hidden;
    background: #0f172a;
  }

  .code-header {
    display: flex;
    gap: 0.5rem;
    padding: 0.75rem 1rem;
    background: rgba(0, 0, 0, 0.3);
  }

  .code-dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
  }

  .code-dot.red { background: #ef4444; }
  .code-dot.yellow { background: #eab308; }
  .code-dot.green { background: #22c55e; }

  .code-content {
    padding: 1rem;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    color: #22d3ee;
    overflow-x: auto;
    white-space: pre;
  }

  /* Skills */
  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
    margin-bottom: 3rem;
  }

  .skill-card {
    padding: 1.5rem;
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(34, 211, 238, 0.1);
    border-radius: 1rem;
  }

  .skill-category {
    font-size: 0.9rem;
    font-weight: 600;
    color: #22d3ee;
    margin-bottom: 1rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .skill-items {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .skill-tag {
    padding: 0.375rem 0.75rem;
    background: rgba(34, 211, 238, 0.1);
    color: #e2e8f0;
    border-radius: 9999px;
    font-size: 0.85rem;
  }

  .cv-section {
    display: flex;
    align-items: center;
    gap: 2rem;
    padding: 2rem;
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(34, 211, 238, 0.1);
    border-radius: 1rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .cv-preview {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    padding: 1.5rem;
    background: #1e293b;
    border-radius: 0.5rem;
    width: 200px;
  }

  .cv-line {
    height: 8px;
    background: rgba(34, 211, 238, 0.2);
    border-radius: 4px;
  }

  .cv-line.short { width: 60%; }
  .cv-line.medium { width: 80%; }

  .download-btn {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 1rem 2rem;
    background: linear-gradient(135deg, #22d3ee, #0891b2);
    color: #020617;
    font-weight: 600;
    border-radius: 9999px;
    transition: all 0.2s;
  }

  .download-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 30px rgba(34, 211, 238, 0.3);
  }

  /* Terminal / Contact */
  .terminal {
    background: #0f172a;
    border: 1px solid rgba(34, 211, 238, 0.2);
    border-radius: 1rem;
    overflow: hidden;
    margin-bottom: 2rem;
  }

  .terminal-header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.75rem 1rem;
    background: rgba(0, 0, 0, 0.3);
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
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    color: #64748b;
  }

  .terminal-body {
    padding: 1.5rem;
  }

  .terminal-line {
    display: flex;
    gap: 0.75rem;
    margin-bottom: 1.5rem;
    font-family: 'JetBrains Mono', monospace;
  }

  .prompt {
    color: #22d3ee;
  }

  .command {
    color: #e2e8f0;
  }

  .contact-form {
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .form-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
  }

  .form-input, .form-textarea {
    padding: 0.875rem 1rem;
    background: rgba(15, 23, 42, 0.6);
    border: 1px solid rgba(34, 211, 238, 0.2);
    border-radius: 0.5rem;
    color: #e2e8f0;
    font-family: inherit;
    font-size: 1rem;
    transition: border-color 0.2s;
  }

  .form-input:focus, .form-textarea:focus {
    outline: none;
    border-color: #22d3ee;
  }

  .form-textarea {
    resize: vertical;
    min-height: 100px;
  }

  .submit-btn {
    align-self: flex-start;
    padding: 0.875rem 1.5rem;
    background: rgba(34, 211, 238, 0.1);
    border: 1px solid rgba(34, 211, 238, 0.3);
    border-radius: 0.5rem;
    color: #22d3ee;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
    transition: all 0.2s;
  }

  .submit-btn:hover {
    background: rgba(34, 211, 238, 0.2);
  }

  .social-links {
    display: flex;
    justify-content: center;
    gap: 2rem;
  }

  .social-link {
    color: #64748b;
    transition: color 0.2s;
  }

  .social-link:hover {
    color: #22d3ee;
  }

  /* Footer */
  .footer {
    padding: 2rem;
    text-align: center;
    color: #64748b;
    font-size: 0.9rem;
    border-top: 1px solid rgba(34, 211, 238, 0.1);
  }

  /* Responsive */
  @media (max-width: 768px) {
    .nav-links {
      display: none;
    }

    .mobile-toggle {
      display: block;
    }

    .hero-name {
      font-size: 2rem;
    }

    .section {
      padding: 4rem 1.5rem;
    }
  }
</style>
