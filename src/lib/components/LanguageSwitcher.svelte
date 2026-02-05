<script>
  import { fly, scale } from 'svelte/transition';
  import { cubicOut } from 'svelte/easing';

  /** @type {{ currentLang: 'en' | 'ka' | 'ru' }} */
  let { currentLang = $bindable('en') } = $props();

  let isOpen = $state(false);

  /**
   * Language options configuration
   */
  const languages = [
    { code: 'en', label: 'EN', fullName: 'English', level: 'C1' },
    { code: 'ka', label: 'KA', fullName: 'ქართული', level: 'Native' },
    { code: 'ru', label: 'RU', fullName: 'Русский', level: 'C1' }
  ];

  /**
   * Get current language data
   */
  function getCurrentLang() {
    return languages.find(l => l.code === currentLang) || languages[0];
  }

  /**
   * Select a language and close dropdown
   * @param {'en' | 'ka' | 'ru'} code
   */
  function selectLang(code) {
    currentLang = code;
    isOpen = false;
  }

  /**
   * Toggle dropdown visibility
   */
  function toggleDropdown() {
    isOpen = !isOpen;
  }

  /**
   * Close dropdown when clicking outside
   * @param {MouseEvent} event
   */
  function handleClickOutside(event) {
    const target = /** @type {HTMLElement} */ (event.target);
    if (!target.closest('.language-switcher')) {
      isOpen = false;
    }
  }
</script>

<svelte:window onclick={handleClickOutside} />

<div class="language-switcher">
  <button 
    class="switcher-trigger"
    onclick={toggleDropdown}
    aria-expanded={isOpen}
    aria-haspopup="listbox"
    aria-label="Select language"
  >
    <span class="trigger-icon">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="12" cy="12" r="10"/>
        <path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
      </svg>
    </span>
    <span class="trigger-label">{getCurrentLang().label}</span>
    <span class="trigger-arrow" class:open={isOpen}>
      <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <polyline points="6 9 12 15 18 9"/>
      </svg>
    </span>
  </button>

  {#if isOpen}
    <div 
      class="switcher-dropdown"
      role="listbox"
      aria-label="Available languages"
      transition:fly={{ y: -10, duration: 200, easing: cubicOut }}
    >
      {#each languages as lang}
        <button
          class="dropdown-option"
          class:active={currentLang === lang.code}
          onclick={() => selectLang(lang.code)}
          role="option"
          aria-selected={currentLang === lang.code}
        >
          <span class="option-label">{lang.label}</span>
          <span class="option-name">{lang.fullName}</span>
          <span class="option-level">{lang.level}</span>
        </button>
      {/each}
    </div>
  {/if}
</div>

<style>
  /* ==========================================================================
     Language Switcher Component
     ========================================================================== */
  .language-switcher {
    position: fixed;
    top: var(--space-4);
    right: var(--space-6);
    z-index: 1001;
  }

  @media (min-width: 768px) {
    .language-switcher {
      top: var(--space-6);
      right: var(--space-8);
    }
  }

  /* --------------------------------------------------------------------------
     Trigger Button
     -------------------------------------------------------------------------- */
  .switcher-trigger {
    display: flex;
    align-items: center;
    gap: var(--space-2);
    padding: var(--space-2) var(--space-3);
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-lg);
    color: var(--color-slate-300);
    font-size: var(--text-sm);
    font-weight: 500;
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
    transition: all var(--duration-fast) var(--ease-out-expo);
  }

  .switcher-trigger:hover {
    border-color: var(--color-cyan-400);
    color: var(--color-cyan-400);
  }

  .trigger-icon {
    display: flex;
    color: var(--color-cyan-400);
  }

  .trigger-label {
    font-family: var(--font-mono);
  }

  .trigger-arrow {
    display: flex;
    transition: transform var(--duration-fast) var(--ease-out-expo);
  }

  .trigger-arrow.open {
    transform: rotate(180deg);
  }

  /* --------------------------------------------------------------------------
     Dropdown Menu
     -------------------------------------------------------------------------- */
  .switcher-dropdown {
    position: absolute;
    top: calc(100% + var(--space-2));
    right: 0;
    min-width: 180px;
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-lg);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
    overflow: hidden;
    box-shadow: var(--shadow-card);
  }

  .dropdown-option {
    width: 100%;
    display: flex;
    align-items: center;
    gap: var(--space-3);
    padding: var(--space-3) var(--space-4);
    color: var(--color-slate-300);
    font-size: var(--text-sm);
    text-align: left;
    transition: all var(--duration-fast) var(--ease-out-expo);
  }

  .dropdown-option:hover {
    background: var(--color-slate-800);
    color: var(--color-slate-100);
  }

  .dropdown-option.active {
    background: var(--color-cyan-subtle);
    color: var(--color-cyan-400);
  }

  .option-label {
    font-family: var(--font-mono);
    font-weight: 600;
    min-width: 28px;
  }

  .option-name {
    flex: 1;
  }

  .option-level {
    font-size: var(--text-xs);
    color: var(--color-slate-500);
    padding: var(--space-1) var(--space-2);
    background: var(--color-slate-800);
    border-radius: var(--radius-sm);
  }

  .dropdown-option.active .option-level {
    background: rgba(34, 211, 238, 0.2);
    color: var(--color-cyan-400);
  }
</style>
