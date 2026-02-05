<script>
  import { onMount } from 'svelte';
  import { fly, fade, scale } from 'svelte/transition';
  import { cubicOut } from 'svelte/easing';

  /** @type {{ t: (key: string) => string }} */
  let { t } = $props();

  let mounted = $state(false);
  let formData = $state({ name: '', email: '', message: '' });
  let isSubmitting = $state(false);
  let submitted = $state(false);
  let focusedField = $state('');

  onMount(() => {
    mounted = true;
  });

  /**
   * Handle form submission
   * @param {Event} event
   */
  async function handleSubmit(event) {
    event.preventDefault();
    isSubmitting = true;
    
    // Simulate form submission
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    isSubmitting = false;
    submitted = true;
    
    // Reset after showing success
    setTimeout(() => {
      formData = { name: '', email: '', message: '' };
      submitted = false;
    }, 3000);
  }

  /**
   * Terminal-style timestamp
   */
  function getTimestamp() {
    const now = new Date();
    return now.toISOString().slice(11, 19);
  }
</script>

<section id="contact" class="contact-section">
  <div class="contact-container">
    <!-- Section Header -->
    <div class="section-header">
      <span class="section-tag">
        <span class="tag-dot"></span>
        Open Channel
      </span>
      <h2 class="section-title">{t('contactTitle')}</h2>
      <p class="section-subtitle">
        Initiating a connection is the first step to building something great together.
      </p>
    </div>

    {#if mounted}
      <div class="contact-grid">
        <!-- Terminal-Style Form -->
        <div 
          class="terminal-wrapper"
          in:fly={{ y: 30, duration: 600, delay: 200, easing: cubicOut }}
        >
          <div class="terminal">
            <!-- Terminal Header -->
            <div class="terminal-header">
              <div class="terminal-buttons">
                <span class="terminal-btn red"></span>
                <span class="terminal-btn yellow"></span>
                <span class="terminal-btn green"></span>
              </div>
              <span class="terminal-title">contact@giorgi.dev ~ zsh</span>
              <div class="terminal-spacer"></div>
            </div>

            <!-- Terminal Body -->
            <div class="terminal-body">
              {#if submitted}
                <div class="success-message" in:scale={{ duration: 400 }}>
                  <div class="success-icon">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <polyline points="20 6 9 17 4 12"/>
                    </svg>
                  </div>
                  <p class="success-text">Signal transmitted successfully</p>
                  <p class="success-subtext">Awaiting response...</p>
                </div>
              {:else}
                <form onsubmit={handleSubmit} class="terminal-form">
                  <!-- Name Field -->
                  <div class="form-line">
                    <span class="prompt">
                      <span class="prompt-user">guest</span>
                      <span class="prompt-at">@</span>
                      <span class="prompt-host">portfolio</span>
                      <span class="prompt-colon">:</span>
                      <span class="prompt-path">~</span>
                      <span class="prompt-symbol">$</span>
                    </span>
                    <span class="command">echo $NAME</span>
                  </div>
                  <div class="input-wrapper" class:focused={focusedField === 'name'}>
                    <input
                      type="text"
                      bind:value={formData.name}
                      onfocus={() => focusedField = 'name'}
                      onblur={() => focusedField = ''}
                      placeholder={t('name')}
                      required
                      class="terminal-input"
                    />
                    <span class="input-cursor" class:visible={focusedField === 'name'}></span>
                  </div>

                  <!-- Email Field -->
                  <div class="form-line">
                    <span class="prompt">
                      <span class="prompt-user">guest</span>
                      <span class="prompt-at">@</span>
                      <span class="prompt-host">portfolio</span>
                      <span class="prompt-colon">:</span>
                      <span class="prompt-path">~</span>
                      <span class="prompt-symbol">$</span>
                    </span>
                    <span class="command">echo $EMAIL</span>
                  </div>
                  <div class="input-wrapper" class:focused={focusedField === 'email'}>
                    <input
                      type="email"
                      bind:value={formData.email}
                      onfocus={() => focusedField = 'email'}
                      onblur={() => focusedField = ''}
                      placeholder={t('email')}
                      required
                      class="terminal-input"
                    />
                    <span class="input-cursor" class:visible={focusedField === 'email'}></span>
                  </div>

                  <!-- Message Field -->
                  <div class="form-line">
                    <span class="prompt">
                      <span class="prompt-user">guest</span>
                      <span class="prompt-at">@</span>
                      <span class="prompt-host">portfolio</span>
                      <span class="prompt-colon">:</span>
                      <span class="prompt-path">~</span>
                      <span class="prompt-symbol">$</span>
                    </span>
                    <span class="command">cat {'>'} message.txt</span>
                  </div>
                  <div class="textarea-wrapper" class:focused={focusedField === 'message'}>
                    <textarea
                      bind:value={formData.message}
                      onfocus={() => focusedField = 'message'}
                      onblur={() => focusedField = ''}
                      placeholder={t('message')}
                      required
                      rows="4"
                      class="terminal-textarea"
                    ></textarea>
                  </div>

                  <!-- Submit -->
                  <div class="form-line submit-line">
                    <span class="prompt">
                      <span class="prompt-user">guest</span>
                      <span class="prompt-at">@</span>
                      <span class="prompt-host">portfolio</span>
                      <span class="prompt-colon">:</span>
                      <span class="prompt-path">~</span>
                      <span class="prompt-symbol">$</span>
                    </span>
                    <button type="submit" class="submit-btn" disabled={isSubmitting}>
                      {#if isSubmitting}
                        <span class="loading-dots">
                          <span>.</span><span>.</span><span>.</span>
                        </span>
                      {:else}
                        <span class="submit-text">./transmit.sh</span>
                        <span class="submit-arrow">
                          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <line x1="5" y1="12" x2="19" y2="12"/>
                            <polyline points="12 5 19 12 12 19"/>
                          </svg>
                        </span>
                      {/if}
                    </button>
                  </div>
                </form>
              {/if}
            </div>

            <!-- Terminal Footer -->
            <div class="terminal-footer">
              <span class="footer-status">
                <span class="status-dot"></span>
                Connected
              </span>
              <span class="footer-time">{getTimestamp()}</span>
            </div>
          </div>
        </div>

        <!-- Contact Info -->
        <div 
          class="contact-info"
          in:fly={{ y: 30, duration: 600, delay: 300, easing: cubicOut }}
        >
          <div class="info-card">
            <h3 class="info-title">Direct Links</h3>
            <div class="info-links">
              <a href="mailto:contact@giorgi.dev" class="info-link">
                <span class="link-icon">
                  <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                    <polyline points="22,6 12,13 2,6"/>
                  </svg>
                </span>
                <span class="link-text">contact@giorgi.dev</span>
              </a>
              <a href="https://github.com/gchantura" target="_blank" rel="noopener noreferrer" class="info-link">
                <span class="link-icon">
                  <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"/>
                  </svg>
                </span>
                <span class="link-text">github.com/gchantura</span>
              </a>
              <a href="https://linkedin.com/in/gchantura" target="_blank" rel="noopener noreferrer" class="info-link">
                <span class="link-icon">
                  <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/>
                    <rect x="2" y="9" width="4" height="12"/>
                    <circle cx="4" cy="4" r="2"/>
                  </svg>
                </span>
                <span class="link-text">linkedin.com/in/gchantura</span>
              </a>
            </div>
          </div>

          <div class="info-card availability">
            <div class="availability-header">
              <span class="availability-dot"></span>
              <span class="availability-text">Available for opportunities</span>
            </div>
            <p class="availability-desc">
              Open to full-time positions, contract work, and interesting collaborations.
            </p>
          </div>
        </div>
      </div>
    {/if}
  </div>
</section>

<style>
  /* ==========================================================================
     Contact Section
     ========================================================================== */
  .contact-section {
    padding: var(--space-24) var(--space-6);
  }

  .contact-container {
    max-width: 1000px;
    margin: 0 auto;
  }

  /* --------------------------------------------------------------------------
     Section Header
     -------------------------------------------------------------------------- */
  .section-header {
    text-align: center;
    margin-bottom: var(--space-16);
  }

  .section-tag {
    display: inline-flex;
    align-items: center;
    gap: var(--space-2);
    padding: var(--space-2) var(--space-4);
    background: var(--color-cyan-subtle);
    border: 1px solid rgba(34, 211, 238, 0.2);
    border-radius: var(--radius-full);
    font-size: var(--text-sm);
    color: var(--color-cyan-400);
    margin-bottom: var(--space-6);
  }

  .tag-dot {
    width: 6px;
    height: 6px;
    background: var(--color-cyan-400);
    border-radius: 50%;
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  .section-title {
    font-size: var(--text-3xl);
    font-weight: 700;
    color: var(--color-slate-100);
    margin-bottom: var(--space-4);
  }

  @media (min-width: 768px) {
    .section-title {
      font-size: var(--text-4xl);
    }
  }

  .section-subtitle {
    font-size: var(--text-lg);
    color: var(--color-slate-400);
    max-width: 500px;
    margin: 0 auto;
  }

  /* --------------------------------------------------------------------------
     Contact Grid
     -------------------------------------------------------------------------- */
  .contact-grid {
    display: grid;
    gap: var(--space-8);
  }

  @media (min-width: 768px) {
    .contact-grid {
      grid-template-columns: 1.2fr 1fr;
    }
  }

  /* --------------------------------------------------------------------------
     Terminal
     -------------------------------------------------------------------------- */
  .terminal-wrapper {
    perspective: 1000px;
  }

  .terminal {
    background: var(--color-slate-900);
    border: 1px solid var(--color-slate-700);
    border-radius: var(--radius-xl);
    overflow: hidden;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
  }

  .terminal-header {
    display: flex;
    align-items: center;
    padding: var(--space-3) var(--space-4);
    background: var(--color-slate-800);
    border-bottom: 1px solid var(--color-slate-700);
  }

  .terminal-buttons {
    display: flex;
    gap: var(--space-2);
  }

  .terminal-btn {
    width: 12px;
    height: 12px;
    border-radius: 50%;
  }

  .terminal-btn.red { background: #ef4444; }
  .terminal-btn.yellow { background: #eab308; }
  .terminal-btn.green { background: #22c55e; }

  .terminal-title {
    flex: 1;
    text-align: center;
    font-family: var(--font-mono);
    font-size: var(--text-xs);
    color: var(--color-slate-400);
  }

  .terminal-spacer {
    width: 52px;
  }

  .terminal-body {
    padding: var(--space-6);
    min-height: 300px;
  }

  .terminal-footer {
    display: flex;
    justify-content: space-between;
    padding: var(--space-2) var(--space-4);
    background: var(--color-slate-800);
    border-top: 1px solid var(--color-slate-700);
    font-family: var(--font-mono);
    font-size: var(--text-xs);
    color: var(--color-slate-500);
  }

  .footer-status {
    display: flex;
    align-items: center;
    gap: var(--space-2);
  }

  .status-dot {
    width: 6px;
    height: 6px;
    background: #22c55e;
    border-radius: 50%;
    animation: pulse 2s ease-in-out infinite;
  }

  /* --------------------------------------------------------------------------
     Terminal Form
     -------------------------------------------------------------------------- */
  .terminal-form {
    display: flex;
    flex-direction: column;
    gap: var(--space-4);
  }

  .form-line {
    display: flex;
    align-items: center;
    gap: var(--space-2);
    flex-wrap: wrap;
  }

  .prompt {
    font-family: var(--font-mono);
    font-size: var(--text-sm);
    display: flex;
    align-items: center;
    flex-shrink: 0;
  }

  .prompt-user { color: #22c55e; }
  .prompt-at { color: var(--color-slate-500); }
  .prompt-host { color: var(--color-cyan-400); }
  .prompt-colon { color: var(--color-slate-500); }
  .prompt-path { color: #a78bfa; }
  .prompt-symbol { color: var(--color-slate-400); margin-left: var(--space-1); }

  .command {
    font-family: var(--font-mono);
    font-size: var(--text-sm);
    color: var(--color-slate-300);
  }

  .input-wrapper,
  .textarea-wrapper {
    position: relative;
    margin-left: var(--space-6);
  }

  .terminal-input,
  .terminal-textarea {
    width: 100%;
    background: transparent;
    border: none;
    font-family: var(--font-mono);
    font-size: var(--text-sm);
    color: var(--color-slate-200);
    padding: var(--space-2) 0;
    outline: none;
    resize: none;
  }

  .terminal-input::placeholder,
  .terminal-textarea::placeholder {
    color: var(--color-slate-600);
  }

  .input-cursor {
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    width: 8px;
    height: 18px;
    background: var(--color-cyan-400);
    opacity: 0;
  }

  .input-cursor.visible {
    animation: blink 1s step-end infinite;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  .input-wrapper.focused,
  .textarea-wrapper.focused {
    border-left: 2px solid var(--color-cyan-400);
    padding-left: var(--space-2);
    margin-left: calc(var(--space-6) - 2px);
  }

  /* Submit Button */
  .submit-line {
    margin-top: var(--space-4);
  }

  .submit-btn {
    display: inline-flex;
    align-items: center;
    gap: var(--space-2);
    padding: var(--space-2) var(--space-4);
    background: var(--color-cyan-500);
    color: var(--color-slate-950);
    font-family: var(--font-mono);
    font-size: var(--text-sm);
    font-weight: 600;
    border-radius: var(--radius-md);
    transition: all var(--duration-fast) var(--ease-out-expo);
  }

  .submit-btn:hover:not(:disabled) {
    background: var(--color-cyan-400);
    transform: translateX(4px);
  }

  .submit-btn:disabled {
    opacity: 0.7;
    cursor: not-allowed;
  }

  .submit-arrow {
    display: flex;
    transition: transform var(--duration-fast) var(--ease-out-expo);
  }

  .submit-btn:hover:not(:disabled) .submit-arrow {
    transform: translateX(4px);
  }

  .loading-dots span {
    animation: loadingDot 1.4s ease-in-out infinite;
  }

  .loading-dots span:nth-child(2) { animation-delay: 0.2s; }
  .loading-dots span:nth-child(3) { animation-delay: 0.4s; }

  @keyframes loadingDot {
    0%, 80%, 100% { opacity: 0.3; }
    40% { opacity: 1; }
  }

  /* Success Message */
  .success-message {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
    min-height: 250px;
    gap: var(--space-4);
    text-align: center;
  }

  .success-icon {
    width: 56px;
    height: 56px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(34, 197, 94, 0.2);
    border: 2px solid #22c55e;
    border-radius: 50%;
    color: #22c55e;
  }

  .success-text {
    font-family: var(--font-mono);
    font-size: var(--text-lg);
    color: var(--color-slate-100);
  }

  .success-subtext {
    font-family: var(--font-mono);
    font-size: var(--text-sm);
    color: var(--color-slate-400);
  }

  /* --------------------------------------------------------------------------
     Contact Info
     -------------------------------------------------------------------------- */
  .contact-info {
    display: flex;
    flex-direction: column;
    gap: var(--space-6);
  }

  .info-card {
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-xl);
    padding: var(--space-6);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
  }

  .info-title {
    font-size: var(--text-lg);
    font-weight: 600;
    color: var(--color-slate-100);
    margin-bottom: var(--space-4);
  }

  .info-links {
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
  }

  .info-link {
    display: flex;
    align-items: center;
    gap: var(--space-3);
    padding: var(--space-3);
    border-radius: var(--radius-md);
    transition: all var(--duration-fast) var(--ease-out-expo);
  }

  .info-link:hover {
    background: var(--color-slate-800);
  }

  .link-icon {
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--color-slate-800);
    border-radius: var(--radius-md);
    color: var(--color-cyan-400);
    flex-shrink: 0;
  }

  .link-text {
    font-size: var(--text-sm);
    color: var(--color-slate-300);
    word-break: break-all;
  }

  .info-link:hover .link-text {
    color: var(--color-cyan-400);
  }

  /* Availability Card */
  .availability {
    border-color: rgba(34, 197, 94, 0.3);
  }

  .availability-header {
    display: flex;
    align-items: center;
    gap: var(--space-2);
    margin-bottom: var(--space-3);
  }

  .availability-dot {
    width: 10px;
    height: 10px;
    background: #22c55e;
    border-radius: 50%;
    animation: pulse 2s ease-in-out infinite;
    box-shadow: 0 0 10px rgba(34, 197, 94, 0.5);
  }

  .availability-text {
    font-weight: 600;
    color: #22c55e;
  }

  .availability-desc {
    font-size: var(--text-sm);
    color: var(--color-slate-400);
    line-height: 1.6;
  }
</style>
