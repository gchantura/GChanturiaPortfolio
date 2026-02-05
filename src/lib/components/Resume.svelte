<script>
  import { onMount } from 'svelte';
  import { fly, scale } from 'svelte/transition';
  import { cubicOut } from 'svelte/easing';

  /** @type {{ t: (key: string) => string }} */
  let { t } = $props();

  let mounted = $state(false);
  let isHovering = $state(false);

  onMount(() => {
    mounted = true;
  });

  /**
   * Skills data organized by category
   */
  const skills = [
    {
      category: 'Frontend',
      items: ['Svelte/SvelteKit', 'React/Next.js', 'TypeScript', 'CSS Architecture']
    },
    {
      category: 'Backend',
      items: ['Node.js', 'Python', 'PostgreSQL', 'REST/GraphQL']
    },
    {
      category: 'AI/ML',
      items: ['LangChain', 'OpenAI API', 'Prompt Engineering', 'Vector DBs']
    },
    {
      category: 'DevOps',
      items: ['Docker', 'CI/CD', 'Cloud Platforms', 'Git']
    }
  ];

  /**
   * Experience timeline
   */
  const experience = [
    {
      role: 'Full Stack Developer & AI Engineer',
      type: 'Independent',
      period: 'Present',
      description: 'Building demand-driven automation systems and AI-powered applications.'
    }
  ];
</script>

<section id="resume" class="resume-section">
  <div class="resume-container">
    <!-- Section Header -->
    <div class="section-header">
      <span class="section-tag">
        <span class="tag-dot"></span>
        Capabilities
      </span>
      <h2 class="section-title">{t('resumeTitle')}</h2>
    </div>

    {#if mounted}
      <div class="resume-grid">
        <!-- Skills Panel -->
        <div 
          class="skills-panel"
          in:fly={{ x: -30, duration: 600, delay: 200, easing: cubicOut }}
        >
          <h3 class="panel-title">Technical Skills</h3>
          <div class="skills-grid">
            {#each skills as skillGroup, i}
              <div 
                class="skill-group"
                in:fly={{ y: 20, duration: 400, delay: 300 + i * 100 }}
              >
                <h4 class="skill-category">{skillGroup.category}</h4>
                <ul class="skill-list">
                  {#each skillGroup.items as skill}
                    <li class="skill-item">
                      <span class="skill-bullet"></span>
                      {skill}
                    </li>
                  {/each}
                </ul>
              </div>
            {/each}
          </div>
        </div>

        <!-- CV Preview Panel -->
        <div 
          class="cv-panel"
          in:fly={{ x: 30, duration: 600, delay: 200, easing: cubicOut }}
        >
          <div class="cv-preview">
            <!-- Decorative CV representation -->
            <div class="cv-document">
              <div class="cv-header-block">
                <div class="cv-avatar"></div>
                <div class="cv-lines">
                  <div class="cv-line long"></div>
                  <div class="cv-line medium"></div>
                </div>
              </div>
              <div class="cv-section-block">
                <div class="cv-line short accent"></div>
                <div class="cv-line full"></div>
                <div class="cv-line full"></div>
                <div class="cv-line medium"></div>
              </div>
              <div class="cv-section-block">
                <div class="cv-line short accent"></div>
                <div class="cv-line full"></div>
                <div class="cv-line full"></div>
              </div>
              <div class="cv-section-block">
                <div class="cv-line short accent"></div>
                <div class="cv-dots">
                  <span class="cv-dot"></span>
                  <span class="cv-dot"></span>
                  <span class="cv-dot"></span>
                  <span class="cv-dot"></span>
                </div>
              </div>
            </div>
            
            <!-- Glow effect -->
            <div class="cv-glow"></div>
          </div>

          <!-- Download Button -->
          <a 
            href="/cv.pdf" 
            class="download-btn"
            onmouseenter={() => isHovering = true}
            onmouseleave={() => isHovering = false}
            download
          >
            <span class="btn-bg"></span>
            <span class="btn-content">
              <svg 
                class="btn-icon" 
                class:animate={isHovering}
                width="20" 
                height="20" 
                viewBox="0 0 24 24" 
                fill="none" 
                stroke="currentColor" 
                stroke-width="2"
              >
                <path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/>
                <polyline points="7 10 12 15 17 10"/>
                <line x1="12" y1="15" x2="12" y2="3"/>
              </svg>
              <span class="btn-text">{t('downloadCV')}</span>
            </span>
            <span class="btn-shimmer"></span>
          </a>

          <!-- Experience -->
          <div class="experience-block">
            {#each experience as exp}
              <div class="experience-item">
                <div class="exp-header">
                  <span class="exp-role">{exp.role}</span>
                  <span class="exp-period">{exp.period}</span>
                </div>
                <span class="exp-type">{exp.type}</span>
                <p class="exp-desc">{exp.description}</p>
              </div>
            {/each}
          </div>
        </div>
      </div>
    {/if}
  </div>
</section>

<style>
  /* ==========================================================================
     Resume Section
     ========================================================================== */
  .resume-section {
    padding: var(--space-24) var(--space-6);
    background: linear-gradient(180deg, transparent 0%, var(--color-slate-900) 50%, transparent 100%);
  }

  .resume-container {
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

  .section-title {
    font-size: var(--text-3xl);
    font-weight: 700;
    color: var(--color-slate-100);
  }

  @media (min-width: 768px) {
    .section-title {
      font-size: var(--text-4xl);
    }
  }

  /* --------------------------------------------------------------------------
     Resume Grid
     -------------------------------------------------------------------------- */
  .resume-grid {
    display: grid;
    gap: var(--space-8);
  }

  @media (min-width: 768px) {
    .resume-grid {
      grid-template-columns: 1fr 1fr;
    }
  }

  /* --------------------------------------------------------------------------
     Skills Panel
     -------------------------------------------------------------------------- */
  .skills-panel {
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-xl);
    padding: var(--space-6);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
  }

  .panel-title {
    font-size: var(--text-lg);
    font-weight: 600;
    color: var(--color-slate-100);
    margin-bottom: var(--space-6);
    padding-bottom: var(--space-4);
    border-bottom: 1px solid var(--glass-border);
  }

  .skills-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--space-6);
  }

  .skill-group {
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
  }

  .skill-category {
    font-size: var(--text-sm);
    font-weight: 600;
    color: var(--color-cyan-400);
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .skill-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: var(--space-2);
  }

  .skill-item {
    display: flex;
    align-items: center;
    gap: var(--space-2);
    font-size: var(--text-sm);
    color: var(--color-slate-300);
  }

  .skill-bullet {
    width: 4px;
    height: 4px;
    background: var(--color-slate-500);
    border-radius: 50%;
    flex-shrink: 0;
  }

  /* --------------------------------------------------------------------------
     CV Panel
     -------------------------------------------------------------------------- */
  .cv-panel {
    display: flex;
    flex-direction: column;
    gap: var(--space-6);
  }

  .cv-preview {
    position: relative;
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-xl);
    padding: var(--space-6);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
    display: flex;
    justify-content: center;
    overflow: hidden;
  }

  .cv-document {
    width: 160px;
    background: var(--color-slate-800);
    border-radius: var(--radius-md);
    padding: var(--space-4);
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
  }

  .cv-header-block {
    display: flex;
    gap: var(--space-3);
    align-items: center;
    padding-bottom: var(--space-3);
    border-bottom: 1px solid var(--color-slate-700);
  }

  .cv-avatar {
    width: 32px;
    height: 32px;
    background: linear-gradient(135deg, var(--color-cyan-500), var(--color-cyan-600));
    border-radius: var(--radius-sm);
    flex-shrink: 0;
  }

  .cv-lines {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: var(--space-2);
  }

  .cv-line {
    height: 6px;
    background: var(--color-slate-700);
    border-radius: var(--radius-sm);
  }

  .cv-line.long { width: 100%; }
  .cv-line.medium { width: 70%; }
  .cv-line.short { width: 40%; }
  .cv-line.full { width: 100%; }
  .cv-line.accent { background: var(--color-cyan-500); height: 4px; }

  .cv-section-block {
    display: flex;
    flex-direction: column;
    gap: var(--space-2);
  }

  .cv-dots {
    display: flex;
    gap: var(--space-2);
    margin-top: var(--space-1);
  }

  .cv-dots .cv-dot {
    width: 16px;
    height: 16px;
    background: var(--color-slate-700);
    border-radius: var(--radius-sm);
  }

  .cv-glow {
    position: absolute;
    width: 200px;
    height: 200px;
    background: radial-gradient(circle, var(--color-cyan-glow) 0%, transparent 70%);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    opacity: 0.3;
    pointer-events: none;
  }

  /* --------------------------------------------------------------------------
     Download Button
     -------------------------------------------------------------------------- */
  .download-btn {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: var(--space-4) var(--space-6);
    border-radius: var(--radius-lg);
    overflow: hidden;
    text-decoration: none;
  }

  .btn-bg {
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, var(--color-cyan-500), var(--color-cyan-600));
    transition: transform var(--duration-normal) var(--ease-out-expo);
  }

  .download-btn:hover .btn-bg {
    transform: scale(1.02);
  }

  .btn-content {
    position: relative;
    display: flex;
    align-items: center;
    gap: var(--space-2);
    color: var(--color-slate-950);
    font-weight: 600;
    font-size: var(--text-sm);
  }

  .btn-icon {
    transition: transform var(--duration-normal) var(--ease-out-expo);
  }

  .btn-icon.animate {
    animation: bounce-down 0.6s ease-in-out infinite;
  }

  @keyframes bounce-down {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(4px); }
  }

  .btn-shimmer {
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      90deg,
      transparent,
      rgba(255, 255, 255, 0.2),
      transparent
    );
    transition: left 0.6s ease;
  }

  .download-btn:hover .btn-shimmer {
    left: 100%;
  }

  /* --------------------------------------------------------------------------
     Experience Block
     -------------------------------------------------------------------------- */
  .experience-block {
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-xl);
    padding: var(--space-6);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
  }

  .experience-item {
    display: flex;
    flex-direction: column;
    gap: var(--space-2);
  }

  .exp-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    flex-wrap: wrap;
    gap: var(--space-2);
  }

  .exp-role {
    font-size: var(--text-base);
    font-weight: 600;
    color: var(--color-slate-100);
  }

  .exp-period {
    font-size: var(--text-sm);
    color: var(--color-cyan-400);
    font-family: var(--font-mono);
  }

  .exp-type {
    font-size: var(--text-sm);
    color: var(--color-slate-400);
  }

  .exp-desc {
    font-size: var(--text-sm);
    color: var(--color-slate-400);
    line-height: 1.6;
  }
</style>
