<script>
  import { onMount } from 'svelte';
  import { fly, slide, fade } from 'svelte/transition';
  import { cubicOut } from 'svelte/easing';

  /** @type {{ t: (key: string) => string }} */
  let { t } = $props();

  let mounted = $state(false);
  let expandedCard = $state(null);

  onMount(() => {
    mounted = true;
  });

  /**
   * Card data with code snippets and architecture descriptions
   */
  const cards = [
    {
      id: 1,
      icon: 'pipeline',
      titleKey: 'card1Title',
      descKey: 'card1Desc',
      color: '#22d3ee',
      codeSnippet: `// Adaptive Pipeline Orchestrator
class PipelineOrchestrator {
  private workers: Worker[] = [];
  private loadThreshold = 0.8;

  async scale(demand: number) {
    const currentLoad = this.getAverageLoad();
    
    if (currentLoad > this.loadThreshold) {
      // Spawn additional workers
      const needed = Math.ceil(
        demand / this.workers.length
      );
      await this.spawnWorkers(needed);
    }
    
    return this.distribute(demand);
  }
}`,
      diagram: [
        { label: 'Input Stream', x: 10, y: 20 },
        { label: 'Load Balancer', x: 40, y: 20 },
        { label: 'Worker Pool', x: 70, y: 20 },
        { label: 'Output', x: 90, y: 20 }
      ]
    },
    {
      id: 2,
      icon: 'architecture',
      titleKey: 'card2Title',
      descKey: 'card2Desc',
      color: '#0891b2',
      codeSnippet: `// Future-Proof Module System
interface ModuleContract<T> {
  version: string;
  migrate: (data: unknown) => T;
  validate: (input: T) => boolean;
}

const createModule = <T>(
  contract: ModuleContract<T>
) => ({
  ...contract,
  // Auto-versioning support
  upgrade: async (legacy: unknown) => {
    const migrated = contract.migrate(legacy);
    if (!contract.validate(migrated)) {
      throw new MigrationError();
    }
    return migrated;
  }
});`,
      diagram: [
        { label: 'Core API', x: 50, y: 15 },
        { label: 'Plugin A', x: 20, y: 50 },
        { label: 'Plugin B', x: 50, y: 50 },
        { label: 'Plugin C', x: 80, y: 50 }
      ]
    },
    {
      id: 3,
      icon: 'demand',
      titleKey: 'card3Title',
      descKey: 'card3Desc',
      color: '#06b6d4',
      codeSnippet: `// Demand-Driven Feature Flag
const featureGate = (
  feature: string,
  userContext: UserContext
) => {
  const demand = analyzeDemand(feature);
  const resources = getAvailableResources();
  
  // Only enable if demand justifies cost
  if (demand.score > resources.threshold) {
    enableFeature(feature, userContext);
    trackAdoption(feature);
    return true;
  }
  
  queueForLaterEvaluation(feature);
  return false;
};`,
      diagram: [
        { label: 'User Signal', x: 15, y: 30 },
        { label: 'Analysis', x: 50, y: 30 },
        { label: 'Decision', x: 85, y: 30 }
      ]
    }
  ];

  /**
   * Toggle card expansion
   * @param {number} id
   */
  function toggleCard(id) {
    expandedCard = expandedCard === id ? null : id;
  }

  /**
   * Get icon SVG based on type
   * @param {string} type
   */
  function getIcon(type) {
    const icons = {
      pipeline: `<path d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"/>`,
      architecture: `<path d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"/>`,
      demand: `<path d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"/>`
    };
    return icons[type] || icons.pipeline;
  }
</script>

<section id="systems" class="systems-section">
  <div class="systems-container">
    <!-- Section Header -->
    <div class="section-header">
      <span class="section-tag">
        <span class="tag-dot"></span>
        Engineering Mindset
      </span>
      <h2 class="section-title">{t('systemsTitle')}</h2>
      <p class="section-subtitle">
        Conceptual frameworks that guide my approach to building scalable, maintainable systems.
      </p>
    </div>

    <!-- Cards Grid -->
    {#if mounted}
      <div class="cards-grid">
        {#each cards as card, i}
          <article 
            class="system-card"
            class:expanded={expandedCard === card.id}
            in:fly={{ y: 40, duration: 600, delay: 200 + i * 100, easing: cubicOut }}
          >
            <button 
              class="card-header"
              onclick={() => toggleCard(card.id)}
              aria-expanded={expandedCard === card.id}
            >
              <div class="card-icon" style="--card-color: {card.color}">
                <svg 
                  width="24" 
                  height="24" 
                  viewBox="0 0 24 24" 
                  fill="none" 
                  stroke="currentColor" 
                  stroke-width="1.5"
                >
                  {@html getIcon(card.icon)}
                </svg>
              </div>
              <div class="card-info">
                <h3 class="card-title">{t(card.titleKey)}</h3>
                <p class="card-desc">{t(card.descKey)}</p>
              </div>
              <div class="card-toggle" class:open={expandedCard === card.id}>
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M12 5v14M5 12h14"/>
                </svg>
              </div>
            </button>

            {#if expandedCard === card.id}
              <div class="card-content" transition:slide={{ duration: 300, easing: cubicOut }}>
                <!-- Code Snippet -->
                <div class="code-block">
                  <div class="code-header">
                    <span class="code-dot red"></span>
                    <span class="code-dot yellow"></span>
                    <span class="code-dot green"></span>
                    <span class="code-filename">system-logic.ts</span>
                  </div>
                  <pre class="code-content"><code>{card.codeSnippet}</code></pre>
                </div>

                <!-- Architecture Diagram -->
                <div class="diagram-block">
                  <div class="diagram-title">Architecture Flow</div>
                  <div class="diagram-canvas">
                    {#each card.diagram as node, nodeIndex}
                      <div 
                        class="diagram-node"
                        style="left: {node.x}%; top: {node.y}%"
                        in:scale={{ duration: 300, delay: nodeIndex * 100 }}
                      >
                        <span class="node-label">{node.label}</span>
                      </div>
                    {/each}
                    <svg class="diagram-lines" viewBox="0 0 100 100" preserveAspectRatio="none">
                      <defs>
                        <linearGradient id="lineGrad-{card.id}" x1="0%" y1="0%" x2="100%" y2="0%">
                          <stop offset="0%" style="stop-color:{card.color};stop-opacity:0.3" />
                          <stop offset="50%" style="stop-color:{card.color};stop-opacity:1" />
                          <stop offset="100%" style="stop-color:{card.color};stop-opacity:0.3" />
                        </linearGradient>
                      </defs>
                      {#if card.diagram.length > 1}
                        {#each card.diagram.slice(0, -1) as node, idx}
                          <line 
                            x1="{node.x}" 
                            y1="{node.y}" 
                            x2="{card.diagram[idx + 1].x}" 
                            y2="{card.diagram[idx + 1].y}"
                            stroke="url(#lineGrad-{card.id})"
                            stroke-width="0.5"
                            stroke-dasharray="2,2"
                          />
                        {/each}
                      {/if}
                    </svg>
                  </div>
                </div>
              </div>
            {/if}
          </article>
        {/each}
      </div>
    {/if}
  </div>
</section>

<style>
  /* ==========================================================================
     Systems Architecture Section
     ========================================================================== */
  .systems-section {
    padding: var(--space-24) var(--space-6);
    position: relative;
  }

  .systems-container {
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
    max-width: 600px;
    margin: 0 auto;
    text-wrap: balance;
  }

  /* --------------------------------------------------------------------------
     Cards Grid
     -------------------------------------------------------------------------- */
  .cards-grid {
    display: flex;
    flex-direction: column;
    gap: var(--space-6);
  }

  /* --------------------------------------------------------------------------
     System Card
     -------------------------------------------------------------------------- */
  .system-card {
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-xl);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
    overflow: hidden;
    transition: all var(--duration-normal) var(--ease-out-expo);
  }

  .system-card:hover {
    border-color: rgba(34, 211, 238, 0.3);
    box-shadow: 0 0 30px rgba(34, 211, 238, 0.1);
  }

  .system-card.expanded {
    border-color: rgba(34, 211, 238, 0.4);
    box-shadow: var(--shadow-glow-sm);
  }

  .card-header {
    width: 100%;
    display: flex;
    align-items: center;
    gap: var(--space-4);
    padding: var(--space-6);
    text-align: left;
    color: var(--color-slate-200);
  }

  .card-icon {
    width: 56px;
    height: 56px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, rgba(34, 211, 238, 0.1), rgba(34, 211, 238, 0.05));
    border: 1px solid rgba(34, 211, 238, 0.2);
    border-radius: var(--radius-lg);
    color: var(--card-color);
    flex-shrink: 0;
  }

  .card-info {
    flex: 1;
    min-width: 0;
  }

  .card-title {
    font-size: var(--text-lg);
    font-weight: 600;
    color: var(--color-slate-100);
    margin-bottom: var(--space-1);
  }

  .card-desc {
    font-size: var(--text-sm);
    color: var(--color-slate-400);
  }

  .card-toggle {
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--color-slate-800);
    border-radius: var(--radius-md);
    color: var(--color-slate-400);
    transition: all var(--duration-fast) var(--ease-out-expo);
    flex-shrink: 0;
  }

  .card-toggle.open {
    background: var(--color-cyan-500);
    color: var(--color-slate-950);
    transform: rotate(45deg);
  }

  /* --------------------------------------------------------------------------
     Card Content (Expanded)
     -------------------------------------------------------------------------- */
  .card-content {
    padding: 0 var(--space-6) var(--space-6);
    display: flex;
    flex-direction: column;
    gap: var(--space-6);
  }

  @media (min-width: 768px) {
    .card-content {
      flex-direction: row;
    }
  }

  /* Code Block */
  .code-block {
    flex: 1;
    background: var(--color-slate-900);
    border: 1px solid var(--color-slate-700);
    border-radius: var(--radius-lg);
    overflow: hidden;
  }

  .code-header {
    display: flex;
    align-items: center;
    gap: var(--space-2);
    padding: var(--space-3) var(--space-4);
    background: var(--color-slate-800);
    border-bottom: 1px solid var(--color-slate-700);
  }

  .code-dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
  }

  .code-dot.red { background: #ef4444; }
  .code-dot.yellow { background: #eab308; }
  .code-dot.green { background: #22c55e; }

  .code-filename {
    margin-left: var(--space-2);
    font-family: var(--font-mono);
    font-size: var(--text-xs);
    color: var(--color-slate-400);
  }

  .code-content {
    padding: var(--space-4);
    font-family: var(--font-mono);
    font-size: var(--text-xs);
    line-height: 1.7;
    color: var(--color-slate-300);
    overflow-x: auto;
    margin: 0;
  }

  /* Diagram Block */
  .diagram-block {
    flex: 1;
    background: var(--color-slate-900);
    border: 1px solid var(--color-slate-700);
    border-radius: var(--radius-lg);
    padding: var(--space-4);
  }

  .diagram-title {
    font-size: var(--text-xs);
    font-weight: 500;
    color: var(--color-slate-400);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: var(--space-4);
  }

  .diagram-canvas {
    position: relative;
    height: 120px;
  }

  .diagram-node {
    position: absolute;
    transform: translate(-50%, -50%);
    padding: var(--space-2) var(--space-3);
    background: var(--glass-bg);
    border: 1px solid var(--color-cyan-400);
    border-radius: var(--radius-md);
    white-space: nowrap;
  }

  .node-label {
    font-size: var(--text-xs);
    color: var(--color-cyan-400);
    font-weight: 500;
  }

  .diagram-lines {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
  }
</style>
