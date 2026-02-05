<script>
  import { onMount } from 'svelte';
  import { fade, fly, scale } from 'svelte/transition';
  import { cubicOut } from 'svelte/easing';

  /** @type {{ t: (key: string) => string }} */
  let { t } = $props();

  let mounted = $state(false);
  let typedText = $state('');
  let showCursor = $state(true);
  
  const fullText = 'Full Stack Developer & AI Engineer';
  
  onMount(() => {
    mounted = true;
    
    // Typing animation
    let i = 0;
    const typeInterval = setInterval(() => {
      if (i < fullText.length) {
        typedText = fullText.slice(0, i + 1);
        i++;
      } else {
        clearInterval(typeInterval);
      }
    }, 60);

    // Cursor blink
    const cursorInterval = setInterval(() => {
      showCursor = !showCursor;
    }, 530);

    return () => {
      clearInterval(typeInterval);
      clearInterval(cursorInterval);
    };
  });
</script>

<section id="hero" class="hero">
  <!-- Animated Background Elements -->
  <div class="hero-bg-elements">
    <div class="orb orb-1"></div>
    <div class="orb orb-2"></div>
    <div class="orb orb-3"></div>
  </div>

  <div class="hero-container">
    {#if mounted}
      <!-- Profile Image with Glassmorphism -->
      <div 
        class="profile-wrapper"
        in:scale={{ duration: 800, delay: 200, easing: cubicOut }}
      >
        <div class="profile-glow"></div>
        <div class="profile-ring"></div>
        <div class="profile-image">
          <div class="profile-placeholder">
            <span class="profile-initials">GT</span>
          </div>
        </div>
        <div class="profile-pulse"></div>
      </div>

      <!-- Hero Content -->
      <div class="hero-content">
        <p 
          class="hero-role"
          in:fly={{ y: 20, duration: 600, delay: 400 }}
        >
          <span class="role-bracket">&lt;</span>
          <span class="typed-text">{typedText}</span>
          <span class="cursor" class:visible={showCursor}>|</span>
          <span class="role-bracket">/&gt;</span>
        </p>

        <h1 
          class="hero-title"
          in:fly={{ y: 30, duration: 600, delay: 600 }}
        >
          <span class="title-name">Giorgi Tchanturia</span>
          <span class="title-tagline">{t('heroTitle')}</span>
        </h1>

        <p 
          class="hero-subtitle"
          in:fly={{ y: 20, duration: 600, delay: 800 }}
        >
          {t('heroSubtitle')}
        </p>

        <div 
          class="hero-cta"
          in:fly={{ y: 20, duration: 600, delay: 1000 }}
        >
          <a href="#systems" class="cta-primary">
            <span class="cta-text">Explore Systems</span>
            <span class="cta-arrow">
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M7 17L17 7M17 7H7M17 7V17"/>
              </svg>
            </span>
          </a>
          <a href="#contact" class="cta-secondary">
            <span class="cta-dot"></span>
            <span>Get in Touch</span>
          </a>
        </div>
      </div>

      <!-- Scroll Indicator -->
      <div 
        class="scroll-indicator"
        in:fade={{ duration: 600, delay: 1200 }}
      >
        <div class="scroll-mouse">
          <div class="scroll-wheel"></div>
        </div>
        <span class="scroll-text">Scroll to explore</span>
      </div>
    {/if}
  </div>
</section>

<style>
  /* ==========================================================================
     Hero Section
     ========================================================================== */
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: var(--space-20) var(--space-6);
    overflow: hidden;
  }

  /* --------------------------------------------------------------------------
     Background Elements
     -------------------------------------------------------------------------- */
  .hero-bg-elements {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }

  .orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0.4;
    animation: float 20s ease-in-out infinite;
  }

  .orb-1 {
    width: 400px;
    height: 400px;
    background: var(--color-cyan-500);
    top: 10%;
    left: 10%;
    animation-delay: 0s;
  }

  .orb-2 {
    width: 300px;
    height: 300px;
    background: var(--color-cyan-600);
    top: 60%;
    right: 10%;
    animation-delay: -5s;
  }

  .orb-3 {
    width: 200px;
    height: 200px;
    background: var(--color-slate-600);
    bottom: 20%;
    left: 30%;
    animation-delay: -10s;
  }

  @keyframes float {
    0%, 100% { transform: translate(0, 0) scale(1); }
    25% { transform: translate(30px, -30px) scale(1.1); }
    50% { transform: translate(-20px, 20px) scale(0.9); }
    75% { transform: translate(20px, 10px) scale(1.05); }
  }

  /* --------------------------------------------------------------------------
     Hero Container
     -------------------------------------------------------------------------- */
  .hero-container {
    position: relative;
    max-width: 1000px;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: var(--space-8);
  }

  /* --------------------------------------------------------------------------
     Profile Image
     -------------------------------------------------------------------------- */
  .profile-wrapper {
    position: relative;
    width: 180px;
    height: 180px;
  }

  @media (min-width: 768px) {
    .profile-wrapper {
      width: 220px;
      height: 220px;
    }
  }

  .profile-glow {
    position: absolute;
    inset: -20px;
    background: radial-gradient(circle, var(--color-cyan-glow) 0%, transparent 70%);
    border-radius: 50%;
    animation: pulse 3s ease-in-out infinite;
  }

  .profile-ring {
    position: absolute;
    inset: -4px;
    border-radius: 50%;
    background: conic-gradient(
      from 0deg,
      var(--color-cyan-400),
      var(--color-cyan-600),
      var(--color-slate-700),
      var(--color-cyan-400)
    );
    animation: rotate 8s linear infinite;
  }

  .profile-image {
    position: absolute;
    inset: 4px;
    border-radius: 50%;
    overflow: hidden;
    background: var(--glass-bg);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
    border: 1px solid var(--glass-border);
  }

  .profile-placeholder {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, var(--color-slate-800), var(--color-slate-900));
  }

  .profile-initials {
    font-family: var(--font-mono);
    font-size: var(--text-4xl);
    font-weight: 600;
    color: var(--color-cyan-400);
  }

  .profile-pulse {
    position: absolute;
    inset: -10px;
    border: 2px solid var(--color-cyan-400);
    border-radius: 50%;
    opacity: 0;
    animation: ripple 3s ease-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 0.4; transform: scale(1); }
    50% { opacity: 0.6; transform: scale(1.05); }
  }

  @keyframes rotate {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }

  @keyframes ripple {
    0% { transform: scale(1); opacity: 0.6; }
    100% { transform: scale(1.5); opacity: 0; }
  }

  /* --------------------------------------------------------------------------
     Hero Content
     -------------------------------------------------------------------------- */
  .hero-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-6);
  }

  .hero-role {
    font-family: var(--font-mono);
    font-size: var(--text-sm);
    color: var(--color-slate-400);
    display: flex;
    align-items: center;
    gap: var(--space-1);
  }

  .role-bracket {
    color: var(--color-cyan-400);
  }

  .typed-text {
    color: var(--color-slate-300);
  }

  .cursor {
    color: var(--color-cyan-400);
    opacity: 0;
    transition: opacity 0.1s;
  }

  .cursor.visible {
    opacity: 1;
  }

  .hero-title {
    display: flex;
    flex-direction: column;
    gap: var(--space-2);
  }

  .title-name {
    font-size: var(--text-4xl);
    font-weight: 700;
    color: var(--color-slate-100);
    letter-spacing: -0.02em;
    line-height: 1.1;
  }

  @media (min-width: 768px) {
    .title-name {
      font-size: var(--text-6xl);
    }
  }

  .title-tagline {
    font-size: var(--text-xl);
    font-weight: 500;
    color: var(--color-cyan-400);
    text-wrap: balance;
  }

  @media (min-width: 768px) {
    .title-tagline {
      font-size: var(--text-2xl);
    }
  }

  .hero-subtitle {
    font-size: var(--text-lg);
    color: var(--color-slate-400);
    max-width: 500px;
    text-wrap: balance;
  }

  /* --------------------------------------------------------------------------
     CTA Buttons
     -------------------------------------------------------------------------- */
  .hero-cta {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: var(--space-4);
    margin-top: var(--space-4);
  }

  .cta-primary {
    display: inline-flex;
    align-items: center;
    gap: var(--space-2);
    padding: var(--space-4) var(--space-6);
    background: linear-gradient(135deg, var(--color-cyan-500), var(--color-cyan-600));
    color: var(--color-slate-950);
    font-weight: 600;
    font-size: var(--text-sm);
    border-radius: var(--radius-lg);
    transition: all var(--duration-normal) var(--ease-out-expo);
    box-shadow: 0 4px 20px var(--color-cyan-glow);
  }

  .cta-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 30px var(--color-cyan-glow);
  }

  .cta-arrow {
    transition: transform var(--duration-fast) var(--ease-out-expo);
  }

  .cta-primary:hover .cta-arrow {
    transform: translate(2px, -2px);
  }

  .cta-secondary {
    display: inline-flex;
    align-items: center;
    gap: var(--space-2);
    padding: var(--space-4) var(--space-6);
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    color: var(--color-slate-200);
    font-weight: 500;
    font-size: var(--text-sm);
    border-radius: var(--radius-lg);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
    transition: all var(--duration-normal) var(--ease-out-expo);
  }

  .cta-secondary:hover {
    border-color: var(--color-cyan-400);
    color: var(--color-cyan-400);
  }

  .cta-dot {
    width: 8px;
    height: 8px;
    background: var(--color-cyan-400);
    border-radius: 50%;
    animation: pulse 2s ease-in-out infinite;
  }

  /* --------------------------------------------------------------------------
     Scroll Indicator
     -------------------------------------------------------------------------- */
  .scroll-indicator {
    position: absolute;
    bottom: var(--space-8);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-3);
    animation: bounce 2s ease-in-out infinite;
  }

  .scroll-mouse {
    width: 24px;
    height: 40px;
    border: 2px solid var(--color-slate-500);
    border-radius: var(--radius-full);
    display: flex;
    justify-content: center;
    padding-top: 8px;
  }

  .scroll-wheel {
    width: 4px;
    height: 8px;
    background: var(--color-cyan-400);
    border-radius: var(--radius-full);
    animation: scroll 2s ease-in-out infinite;
  }

  .scroll-text {
    font-size: var(--text-xs);
    color: var(--color-slate-500);
    text-transform: uppercase;
    letter-spacing: 0.1em;
  }

  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(10px); }
  }

  @keyframes scroll {
    0%, 100% { opacity: 1; transform: translateY(0); }
    50% { opacity: 0.5; transform: translateY(6px); }
  }
</style>
