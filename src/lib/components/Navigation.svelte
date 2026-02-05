<script>
  import { fly, fade } from 'svelte/transition';
  
  /** @type {{ currentLang: string, t: (key: string) => string }} */
  let { currentLang, t } = $props();

  let scrolled = $state(false);
  let mobileMenuOpen = $state(false);

  /** Navigation links configuration */
  const navLinks = [
    { href: '#hero', label: 'Home' },
    { href: '#systems', label: 'Systems' },
    { href: '#resume', label: 'Resume' },
    { href: '#contact', label: 'Contact' }
  ];

  /**
   * Handle scroll event to add background to nav
   */
  function handleScroll() {
    scrolled = window.scrollY > 50;
  }

  /**
   * Toggle mobile menu
   */
  function toggleMenu() {
    mobileMenuOpen = !mobileMenuOpen;
  }

  /**
   * Close mobile menu when link is clicked
   */
  function closeMenu() {
    mobileMenuOpen = false;
  }
</script>

<svelte:window onscroll={handleScroll} />

<nav class="navigation" class:scrolled>
  <div class="nav-container">
    <!-- Logo / Name -->
    <a href="#hero" class="nav-logo">
      <span class="logo-bracket">[</span>
      <span class="logo-text">GT</span>
      <span class="logo-bracket">]</span>
    </a>

    <!-- Desktop Navigation -->
    <div class="nav-links">
      {#each navLinks as link}
        <a href={link.href} class="nav-link">
          <span class="nav-link-text">{link.label}</span>
          <span class="nav-link-underline"></span>
        </a>
      {/each}
    </div>

    <!-- Mobile Menu Button -->
    <button 
      class="mobile-menu-btn" 
      onclick={toggleMenu}
      aria-label="Toggle navigation menu"
      aria-expanded={mobileMenuOpen}
    >
      <span class="menu-line" class:open={mobileMenuOpen}></span>
      <span class="menu-line" class:open={mobileMenuOpen}></span>
      <span class="menu-line" class:open={mobileMenuOpen}></span>
    </button>
  </div>

  <!-- Mobile Navigation Overlay -->
  {#if mobileMenuOpen}
    <div 
      class="mobile-nav-overlay" 
      transition:fade={{ duration: 200 }}
      role="presentation"
    >
      <div class="mobile-nav-content" transition:fly={{ y: -20, duration: 300 }}>
        {#each navLinks as link, i}
          <a 
            href={link.href} 
            class="mobile-nav-link"
            onclick={closeMenu}
            style="animation-delay: {i * 50}ms"
          >
            {link.label}
          </a>
        {/each}
      </div>
    </div>
  {/if}
</nav>

<style>
  /* ==========================================================================
     Navigation Component Styles
     ========================================================================== */
  .navigation {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    padding: var(--space-4) var(--space-6);
    transition: all var(--duration-normal) var(--ease-out-expo);
  }

  .navigation.scrolled {
    background: var(--glass-bg);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
    border-bottom: 1px solid var(--glass-border);
  }

  .nav-container {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  /* --------------------------------------------------------------------------
     Logo
     -------------------------------------------------------------------------- */
  .nav-logo {
    font-family: var(--font-mono);
    font-size: var(--text-xl);
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: var(--space-1);
    transition: transform var(--duration-fast) var(--ease-out-expo);
  }

  .nav-logo:hover {
    transform: scale(1.05);
  }

  .logo-bracket {
    color: var(--color-cyan-400);
  }

  .logo-text {
    color: var(--color-slate-100);
  }

  /* --------------------------------------------------------------------------
     Desktop Navigation Links
     -------------------------------------------------------------------------- */
  .nav-links {
    display: none;
    align-items: center;
    gap: var(--space-8);
  }

  @media (min-width: 768px) {
    .nav-links {
      display: flex;
    }
  }

  .nav-link {
    position: relative;
    font-size: var(--text-sm);
    font-weight: 500;
    color: var(--color-slate-300);
    padding: var(--space-2) 0;
    transition: color var(--duration-fast) var(--ease-out-expo);
  }

  .nav-link:hover {
    color: var(--color-cyan-400);
  }

  .nav-link-underline {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--color-cyan-400), var(--color-cyan-600));
    transition: width var(--duration-normal) var(--ease-out-expo);
  }

  .nav-link:hover .nav-link-underline {
    width: 100%;
  }

  /* --------------------------------------------------------------------------
     Mobile Menu Button
     -------------------------------------------------------------------------- */
  .mobile-menu-btn {
    display: flex;
    flex-direction: column;
    gap: 5px;
    padding: var(--space-2);
    z-index: 1001;
  }

  @media (min-width: 768px) {
    .mobile-menu-btn {
      display: none;
    }
  }

  .menu-line {
    width: 24px;
    height: 2px;
    background: var(--color-slate-200);
    transition: all var(--duration-normal) var(--ease-out-expo);
    transform-origin: center;
  }

  .menu-line.open:nth-child(1) {
    transform: translateY(7px) rotate(45deg);
  }

  .menu-line.open:nth-child(2) {
    opacity: 0;
    transform: scaleX(0);
  }

  .menu-line.open:nth-child(3) {
    transform: translateY(-7px) rotate(-45deg);
  }

  /* --------------------------------------------------------------------------
     Mobile Navigation Overlay
     -------------------------------------------------------------------------- */
  .mobile-nav-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: var(--color-slate-950);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 999;
  }

  .mobile-nav-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-8);
  }

  .mobile-nav-link {
    font-size: var(--text-3xl);
    font-weight: 600;
    color: var(--color-slate-200);
    transition: color var(--duration-fast) var(--ease-out-expo);
    animation: fadeInUp var(--duration-normal) var(--ease-out-expo) both;
  }

  .mobile-nav-link:hover {
    color: var(--color-cyan-400);
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
