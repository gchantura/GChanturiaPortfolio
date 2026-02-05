<script>
	import { fade, fly, scale } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';

	let currentLang = $state('en');
	let expandedCard = $state(null);
	let isContactFormVisible = $state(false);

	const translations = {
		en: {
			hero: {
				title: 'Giorgi Tchanturia',
				subtitle: 'Engineering Demand-Driven Automation',
				tagline: "I don't just write code; I build future-proof logic."
			},
			nav: {
				systems: 'Systems Architecture',
				resume: 'Resume',
				contact: 'Connect'
			},
			systems: {
				title: 'Systems Architecture & Logic',
				subtitle: 'Engineering Mindset in Action',
				cards: [
					{
						id: 'adaptive',
						title: 'Adaptive AI Pipelines',
						description: 'Logic for autonomous workflow scaling',
						details: 'Building intelligent systems that scale automatically based on demand, optimizing resource allocation in real-time.',
						codeSnippet: `// Adaptive scaling logic
const scaleWorkflow = (demand) => {
  const resources = calculateOptimal(demand);
  return orchestrate({
    workers: resources.compute,
    memory: resources.allocation,
    priority: demand.urgency
  });
};`
					},
					{
						id: 'architecture',
						title: 'Future-Proof Architectures',
						description: 'Designing systems that anticipate automation',
						details: 'Creating modular, extensible systems with abstraction layers that adapt to evolving requirements without major refactoring.',
						codeSnippet: `// Modular architecture pattern
class SystemCore {
  constructor(config) {
    this.modules = new Map();
    this.eventBus = new EventEmitter();
  }

  extend(module) {
    this.modules.set(module.id, module);
    return this.orchestrate();
  }
}`
					},
					{
						id: 'demand',
						title: 'Demand-Driven Development',
						description: 'Creating solutions based on necessity',
						details: 'Analyzing real-world problems first, then engineering elegant solutions that address core needs rather than following trends.',
						codeSnippet: `// Problem-first approach
const solveProblem = async (context) => {
  const analysis = await analyzeDemand(context);
  const solution = await architect(analysis);

  return {
    approach: solution.strategy,
    implementation: solution.blueprint,
    scalability: solution.futureProof
  };
};`
					}
				]
			},
			resume: {
				title: 'Professional Resume',
				subtitle: 'Full Stack Developer & AI Engineer',
				download: 'Download CV',
				preview: 'Preview Resume'
			},
			contact: {
				title: 'Let\'s Connect',
				subtitle: 'Open to opportunities and collaborations',
				form: {
					name: 'Your Name',
					email: 'Your Email',
					message: 'Your Message',
					submit: 'Send Message'
				},
				terminal: {
					line1: '> Initializing connection...',
					line2: '> Email: giorgio.tchanturia@example.com',
					line3: '> LinkedIn: /in/giorgi-tchanturia',
					line4: '> GitHub: /gtchanturia',
					line5: '> Status: Available for opportunities',
					prompt: '> _'
				}
			}
		},
		ka: {
			hero: {
				title: 'გიორგი ჭანტურია',
				subtitle: 'მოთხოვნაზე დაფუძნებული ავტომატიზაციის ინჟინერია',
				tagline: 'მე არ ვწერ უბრალოდ კოდს; ვქმნი მომავლის ლოგიკას.'
			},
			nav: {
				systems: 'სისტემური არქიტექტურა',
				resume: 'რეზიუმე',
				contact: 'დაკავშირება'
			},
			systems: {
				title: 'სისტემური არქიტექტურა და ლოგიკა',
				subtitle: 'საინჟინრო აზროვნება მოქმედებაში',
				cards: [
					{
						id: 'adaptive',
						title: 'ადაპტური AI Pipeline-ები',
						description: 'ავტონომიური სამუშაო პროცესების სკალირების ლოგიკა',
						details: 'ინტელექტუალური სისტემების აშენება, რომლებიც ავტომატურად სკალირდება მოთხოვნის მიხედვით.',
						codeSnippet: `// ადაპტური სკალირების ლოგიკა
const scaleWorkflow = (demand) => {
  const resources = calculateOptimal(demand);
  return orchestrate({
    workers: resources.compute,
    memory: resources.allocation,
    priority: demand.urgency
  });
};`
					},
					{
						id: 'architecture',
						title: 'მომავლის არქიტექტურები',
						description: 'სისტემების დიზაინი, რომელიც ავტომატიზაციას ითვალისწინებს',
						details: 'მოდულური, გაფართოებადი სისტემების შექმნა, რომლებიც ადაპტირდება განვითარებად მოთხოვნებზე.',
						codeSnippet: `// მოდულური არქიტექტურის პატერნი
class SystemCore {
  constructor(config) {
    this.modules = new Map();
    this.eventBus = new EventEmitter();
  }

  extend(module) {
    this.modules.set(module.id, module);
    return this.orchestrate();
  }
}`
					},
					{
						id: 'demand',
						title: 'მოთხოვნაზე ორიენტირებული განვითარება',
						description: 'გადაწყვეტილებების შექმნა აუცილებლობის საფუძველზე',
						details: 'რეალურ პრობლემების ანალიზი პირველ რიგში, შემდეგ კი ელეგანტური გადაწყვეტილებების ინჟინერია.',
						codeSnippet: `// პრობლემაზე ორიენტირებული მიდგომა
const solveProblem = async (context) => {
  const analysis = await analyzeDemand(context);
  const solution = await architect(analysis);

  return {
    approach: solution.strategy,
    implementation: solution.blueprint,
    scalability: solution.futureProof
  };
};`
					}
				]
			},
			resume: {
				title: 'პროფესიული რეზიუმე',
				subtitle: 'Full Stack დეველოპერი და AI ინჟინერი',
				download: 'CV-ის ჩამოტვირთვა',
				preview: 'რეზიუმეს გადახედვა'
			},
			contact: {
				title: 'მოდი დავუკავშირდეთ',
				subtitle: 'ღია ვარ შესაძლებლობებისა და თანამშრომლობისთვის',
				form: {
					name: 'თქვენი სახელი',
					email: 'თქვენი ელფოსტა',
					message: 'თქვენი შეტყობინება',
					submit: 'გაგზავნა'
				},
				terminal: {
					line1: '> კავშირის ინიციალიზაცია...',
					line2: '> ელფოსტა: giorgio.tchanturia@example.com',
					line3: '> LinkedIn: /in/giorgi-tchanturia',
					line4: '> GitHub: /gtchanturia',
					line5: '> სტატუსი: ხელმისაწვდომი შესაძლებლობებისთვის',
					prompt: '> _'
				}
			}
		},
		ru: {
			hero: {
				title: 'Гиорги Чантурия',
				subtitle: 'Инженерия Автоматизации на Основе Спроса',
				tagline: 'Я не просто пишу код; я создаю логику будущего.'
			},
			nav: {
				systems: 'Системная Архитектура',
				resume: 'Резюме',
				contact: 'Связаться'
			},
			systems: {
				title: 'Системная Архитектура и Логика',
				subtitle: 'Инженерное мышление в действии',
				cards: [
					{
						id: 'adaptive',
						title: 'Адаптивные AI Pipeline',
						description: 'Логика автономного масштабирования рабочих процессов',
						details: 'Создание интеллектуальных систем, которые масштабируются автоматически на основе спроса.',
						codeSnippet: `// Логика адаптивного масштабирования
const scaleWorkflow = (demand) => {
  const resources = calculateOptimal(demand);
  return orchestrate({
    workers: resources.compute,
    memory: resources.allocation,
    priority: demand.urgency
  });
};`
					},
					{
						id: 'architecture',
						title: 'Архитектуры Будущего',
						description: 'Проектирование систем с учетом автоматизации',
						details: 'Создание модульных, расширяемых систем, адаптирующихся к развивающимся требованиям.',
						codeSnippet: `// Паттерн модульной архитектуры
class SystemCore {
  constructor(config) {
    this.modules = new Map();
    this.eventBus = new EventEmitter();
  }

  extend(module) {
    this.modules.set(module.id, module);
    return this.orchestrate();
  }
}`
					},
					{
						id: 'demand',
						title: 'Разработка на Основе Спроса',
						description: 'Создание решений на основе необходимости',
						details: 'Анализ реальных проблем в первую очередь, затем инженерия элегантных решений.',
						codeSnippet: `// Подход с акцентом на проблему
const solveProblem = async (context) => {
  const analysis = await analyzeDemand(context);
  const solution = await architect(analysis);

  return {
    approach: solution.strategy,
    implementation: solution.blueprint,
    scalability: solution.futureProof
  };
};`
					}
				]
			},
			resume: {
				title: 'Профессиональное Резюме',
				subtitle: 'Full Stack Разработчик и AI Инженер',
				download: 'Скачать CV',
				preview: 'Просмотр резюме'
			},
			contact: {
				title: 'Давайте Свяжемся',
				subtitle: 'Открыт для возможностей и сотрудничества',
				form: {
					name: 'Ваше имя',
					email: 'Ваш email',
					message: 'Ваше сообщение',
					submit: 'Отправить'
				},
				terminal: {
					line1: '> Инициализация соединения...',
					line2: '> Email: giorgio.tchanturia@example.com',
					line3: '> LinkedIn: /in/giorgi-tchanturia',
					line4: '> GitHub: /gtchanturia',
					line5: '> Статус: Доступен для предложений',
					prompt: '> _'
				}
			}
		}
	};

	$effect(() => {
		const stored = localStorage.getItem('portfolio-lang');
		if (stored && ['en', 'ka', 'ru'].includes(stored)) {
			currentLang = stored;
		}
	});

	function switchLanguage(lang) {
		currentLang = lang;
		localStorage.setItem('portfolio-lang', lang);
	}

	function toggleCard(cardId) {
		expandedCard = expandedCard === cardId ? null : cardId;
	}

	function scrollToSection(sectionId) {
		document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' });
	}

	$effect(() => {
		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						entry.target.classList.add('visible');
					}
				});
			},
			{ threshold: 0.1 }
		);

		document.querySelectorAll('.reveal').forEach((el) => observer.observe(el));

		return () => observer.disconnect();
	});

	const t = $derived(translations[currentLang]);
</script>

<main class="portfolio">
	<nav class="main-nav">
		<div class="nav-content">
			<div class="nav-links">
				<button onclick={() => scrollToSection('systems')} class="nav-link">
					{t.nav.systems}
				</button>
				<button onclick={() => scrollToSection('resume')} class="nav-link">
					{t.nav.resume}
				</button>
				<button onclick={() => scrollToSection('contact')} class="nav-link">
					{t.nav.contact}
				</button>
			</div>

			<div class="lang-switcher">
				<button
					class="lang-btn"
					class:active={currentLang === 'en'}
					onclick={() => switchLanguage('en')}
				>
					EN
				</button>
				<button
					class="lang-btn"
					class:active={currentLang === 'ka'}
					onclick={() => switchLanguage('ka')}
				>
					KA
				</button>
				<button
					class="lang-btn"
					class:active={currentLang === 'ru'}
					onclick={() => switchLanguage('ru')}
				>
					RU
				</button>
			</div>
		</div>
	</nav>

	<section class="hero">
		<div class="hero-content">
			<div class="hero-photo">
				<div class="photo-glow"></div>
				<div class="photo-container">
					<div class="photo-placeholder">GT</div>
				</div>
			</div>

			<div class="hero-text">
				<h1 class="hero-title">
					{t.hero.title}
				</h1>
				<p class="hero-subtitle">{t.hero.subtitle}</p>
				<p class="hero-tagline">{t.hero.tagline}</p>
			</div>
		</div>

		<div class="scroll-indicator">
			<div class="scroll-line"></div>
		</div>
	</section>

	<section id="systems" class="systems-section reveal">
		<div class="section-header">
			<h2 class="section-title">{t.systems.title}</h2>
			<p class="section-subtitle">{t.systems.subtitle}</p>
		</div>

		<div class="systems-grid">
			{#each t.systems.cards as card (card.id)}
				<div class="system-card" class:expanded={expandedCard === card.id}>
					<button
						class="card-front"
						onclick={() => toggleCard(card.id)}
						aria-expanded={expandedCard === card.id}
					>
						<div class="card-icon">
							<div class="icon-pulse"></div>
						</div>
						<h3 class="card-title">{card.title}</h3>
						<p class="card-description">{card.description}</p>
						<div class="card-expand-hint">
							{expandedCard === card.id ? '−' : '+'}
						</div>
					</button>

					{#if expandedCard === card.id}
						<div class="card-back" transition:fly={{ y: 20, duration: 400, easing: quintOut }}>
							<p class="card-details">{card.details}</p>
							<div class="code-snippet">
								<pre><code>{card.codeSnippet}</code></pre>
							</div>
						</div>
					{/if}
				</div>
			{/each}
		</div>
	</section>

	<section id="resume" class="resume-section reveal">
		<div class="section-header">
			<h2 class="section-title">{t.resume.title}</h2>
			<p class="section-subtitle">{t.resume.subtitle}</p>
		</div>

		<div class="resume-container">
			<div class="resume-preview">
				<div class="resume-placeholder">
					<div class="resume-icon">
						<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
							<path
								d="M14 2H6C5.46957 2 4.96086 2.21071 4.58579 2.58579C4.21071 2.96086 4 3.46957 4 4V20C4 20.5304 4.21071 21.0391 4.58579 21.4142C4.96086 21.7893 5.46957 22 6 22H18C18.5304 22 19.0391 21.7893 19.4142 21.4142C19.7893 21.0391 20 20.5304 20 20V8L14 2Z"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
							/>
							<path d="M14 2V8H20" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
							<path d="M16 13H8" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
							<path d="M16 17H8" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
							<path d="M10 9H9H8" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
						</svg>
					</div>
					<p>{t.resume.preview}</p>
				</div>
			</div>

			<a href="/cv-giorgi-tchanturia.pdf" download class="download-btn">
				<span class="btn-text">{t.resume.download}</span>
				<span class="btn-icon">
					<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
						<path
							d="M21 15V19C21 19.5304 20.7893 20.0391 20.4142 20.4142C20.0391 20.7893 19.5304 21 19 21H5C4.46957 21 3.96086 20.7893 3.58579 20.4142C3.21071 20.0391 3 19.5304 3 19V15"
							stroke="currentColor"
							stroke-width="2"
							stroke-linecap="round"
							stroke-linejoin="round"
						/>
						<path d="M7 10L12 15L17 10" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
						<path d="M12 15V3" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
					</svg>
				</span>
			</a>
		</div>
	</section>

	<section id="contact" class="contact-section reveal">
		<div class="section-header">
			<h2 class="section-title">{t.contact.title}</h2>
			<p class="section-subtitle">{t.contact.subtitle}</p>
		</div>

		<div class="terminal">
			<div class="terminal-header">
				<div class="terminal-controls">
					<span class="control control-close"></span>
					<span class="control control-minimize"></span>
					<span class="control control-maximize"></span>
				</div>
				<div class="terminal-title">contact.sh</div>
			</div>

			<div class="terminal-body">
				<div class="terminal-line">{t.contact.terminal.line1}</div>
				<div class="terminal-line delay-1">{t.contact.terminal.line2}</div>
				<div class="terminal-line delay-2">{t.contact.terminal.line3}</div>
				<div class="terminal-line delay-3">{t.contact.terminal.line4}</div>
				<div class="terminal-line delay-4">{t.contact.terminal.line5}</div>
				<div class="terminal-line delay-5 terminal-prompt">{t.contact.terminal.prompt}</div>
			</div>
		</div>
	</section>

	<footer class="footer">
		<p>Built with SvelteKit & Pure CSS Architecture</p>
		<p class="footer-year">© 2024 Giorgi Tchanturia</p>
	</footer>
</main>

<style>
	:global(:root) {
		--color-slate: #0f172a;
		--color-slate-light: #1e293b;
		--color-slate-lighter: #334155;
		--color-cyan: #06b6d4;
		--color-cyan-bright: #22d3ee;
		--color-cyan-dim: #0891b2;
		--color-text-primary: #f8fafc;
		--color-text-secondary: #cbd5e1;
		--color-text-muted: #64748b;
		--color-glow: rgba(6, 182, 212, 0.4);
		--color-glow-intense: rgba(34, 211, 238, 0.6);

		--spacing-xs: 0.5rem;
		--spacing-sm: 1rem;
		--spacing-md: 1.5rem;
		--spacing-lg: 2rem;
		--spacing-xl: 3rem;
		--spacing-2xl: 4rem;
		--spacing-3xl: 6rem;

		--font-size-sm: 0.875rem;
		--font-size-base: 1rem;
		--font-size-lg: 1.125rem;
		--font-size-xl: 1.5rem;
		--font-size-2xl: 2rem;
		--font-size-3xl: 3rem;
		--font-size-4xl: 4rem;

		--transition-fast: 0.2s ease;
		--transition-base: 0.3s ease;
		--transition-slow: 0.5s ease;

		--border-radius-sm: 0.5rem;
		--border-radius-md: 1rem;
		--border-radius-lg: 1.5rem;

		--glass-bg: rgba(30, 41, 59, 0.4);
		--glass-border: rgba(148, 163, 184, 0.1);

		--shadow-glow: 0 0 20px var(--color-glow);
		--shadow-glow-intense: 0 0 40px var(--color-glow-intense);
	}

	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	:global(body) {
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
		background: var(--color-slate);
		color: var(--color-text-primary);
		line-height: 1.6;
		overflow-x: hidden;
		scroll-behavior: smooth;
	}

	:global(button) {
		border: none;
		background: none;
		cursor: pointer;
		font-family: inherit;
	}

	.portfolio {
		position: relative;
		min-height: 100vh;
	}

	.main-nav {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 1000;
		padding: var(--spacing-md) var(--spacing-xl);
		background: rgba(15, 23, 42, 0.8);
		backdrop-filter: blur(10px);
		border-bottom: 1px solid var(--glass-border);
	}

	.nav-content {
		max-width: 1400px;
		margin: 0 auto;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.nav-links {
		display: flex;
		gap: var(--spacing-lg);
	}

	.nav-link {
		color: var(--color-text-secondary);
		font-size: var(--font-size-base);
		font-weight: 500;
		transition: color var(--transition-fast);
		position: relative;
	}

	.nav-link::after {
		content: '';
		position: absolute;
		bottom: -4px;
		left: 0;
		width: 0;
		height: 2px;
		background: var(--color-cyan);
		transition: width var(--transition-base);
	}

	.nav-link:hover {
		color: var(--color-cyan-bright);
	}

	.nav-link:hover::after {
		width: 100%;
	}

	.lang-switcher {
		display: flex;
		gap: var(--spacing-xs);
		padding: 0.25rem;
		background: var(--glass-bg);
		border-radius: var(--border-radius-sm);
		border: 1px solid var(--glass-border);
	}

	.lang-btn {
		padding: 0.5rem 1rem;
		color: var(--color-text-secondary);
		font-size: var(--font-size-sm);
		font-weight: 600;
		border-radius: 0.375rem;
		transition: all var(--transition-fast);
	}

	.lang-btn:hover {
		color: var(--color-cyan-bright);
		background: rgba(6, 182, 212, 0.1);
	}

	.lang-btn.active {
		color: var(--color-slate);
		background: var(--color-cyan);
		box-shadow: var(--shadow-glow);
	}

	.hero {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: var(--spacing-3xl) var(--spacing-xl);
		position: relative;
		overflow: hidden;
	}

	.hero::before {
		content: '';
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 800px;
		height: 800px;
		background: radial-gradient(circle, var(--color-glow) 0%, transparent 70%);
		animation: pulse-glow 4s ease-in-out infinite;
		pointer-events: none;
	}

	@keyframes pulse-glow {
		0%, 100% {
			opacity: 0.3;
			transform: translate(-50%, -50%) scale(1);
		}
		50% {
			opacity: 0.6;
			transform: translate(-50%, -50%) scale(1.1);
		}
	}

	.hero-content {
		max-width: 1200px;
		text-align: center;
		position: relative;
		z-index: 1;
	}

	.hero-photo {
		position: relative;
		margin: 0 auto var(--spacing-xl);
		width: 200px;
		height: 200px;
	}

	.photo-glow {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 240px;
		height: 240px;
		background: var(--color-cyan);
		border-radius: 50%;
		filter: blur(40px);
		opacity: 0.3;
		animation: photo-pulse 3s ease-in-out infinite;
	}

	@keyframes photo-pulse {
		0%, 100% {
			opacity: 0.3;
			transform: translate(-50%, -50%) scale(1);
		}
		50% {
			opacity: 0.5;
			transform: translate(-50%, -50%) scale(1.1);
		}
	}

	.photo-container {
		position: relative;
		width: 200px;
		height: 200px;
		border-radius: 50%;
		overflow: hidden;
		border: 3px solid var(--color-cyan);
		box-shadow: var(--shadow-glow-intense);
		background: linear-gradient(135deg, var(--color-slate-light), var(--color-slate-lighter));
	}

	.photo-placeholder {
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: var(--font-size-3xl);
		font-weight: 700;
		color: var(--color-cyan-bright);
		background: linear-gradient(135deg, rgba(6, 182, 212, 0.1), rgba(34, 211, 238, 0.2));
	}

	.hero-text {
		animation: fade-in-up 1s ease-out;
	}

	@keyframes fade-in-up {
		from {
			opacity: 0;
			transform: translateY(30px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.hero-title {
		font-size: clamp(var(--font-size-3xl), 5vw, var(--font-size-4xl));
		font-weight: 700;
		margin-bottom: var(--spacing-md);
		background: linear-gradient(135deg, var(--color-text-primary), var(--color-cyan-bright));
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.hero-subtitle {
		font-size: clamp(var(--font-size-lg), 2vw, var(--font-size-xl));
		color: var(--color-cyan);
		margin-bottom: var(--spacing-md);
		font-weight: 600;
	}

	.hero-tagline {
		font-size: var(--font-size-lg);
		color: var(--color-text-secondary);
		max-width: 600px;
		margin: 0 auto;
		line-height: 1.8;
	}

	.scroll-indicator {
		position: absolute;
		bottom: var(--spacing-xl);
		left: 50%;
		transform: translateX(-50%);
		width: 2px;
		height: 60px;
		background: var(--glass-border);
		border-radius: 2px;
		overflow: hidden;
	}

	.scroll-line {
		width: 100%;
		height: 30px;
		background: var(--color-cyan);
		animation: scroll-animation 2s ease-in-out infinite;
	}

	@keyframes scroll-animation {
		0% {
			transform: translateY(-100%);
		}
		100% {
			transform: translateY(200%);
		}
	}

	.systems-section,
	.resume-section,
	.contact-section {
		padding: var(--spacing-3xl) var(--spacing-xl);
		max-width: 1400px;
		margin: 0 auto;
	}

	.reveal {
		opacity: 0;
		transform: translateY(50px);
		transition: opacity 0.8s ease, transform 0.8s ease;
	}

	.reveal.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.section-header {
		text-align: center;
		margin-bottom: var(--spacing-3xl);
	}

	.section-title {
		font-size: clamp(var(--font-size-2xl), 4vw, var(--font-size-3xl));
		font-weight: 700;
		margin-bottom: var(--spacing-sm);
		background: linear-gradient(135deg, var(--color-text-primary), var(--color-cyan));
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.section-subtitle {
		font-size: var(--font-size-lg);
		color: var(--color-text-secondary);
	}

	.systems-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
		gap: var(--spacing-lg);
	}

	.system-card {
		background: var(--glass-bg);
		border: 1px solid var(--glass-border);
		border-radius: var(--border-radius-lg);
		padding: var(--spacing-xl);
		backdrop-filter: blur(10px);
		transition: all var(--transition-base);
		cursor: pointer;
		position: relative;
		overflow: hidden;
	}

	.system-card::before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		height: 2px;
		background: linear-gradient(90deg, transparent, var(--color-cyan), transparent);
		transform: translateX(-100%);
		transition: transform var(--transition-slow);
	}

	.system-card:hover::before {
		transform: translateX(100%);
	}

	.system-card:hover {
		transform: translateY(-8px);
		border-color: var(--color-cyan);
		box-shadow: var(--shadow-glow);
	}

	.system-card.expanded {
		grid-column: span 1;
	}

	.card-front {
		position: relative;
		width: 100%;
		text-align: left;
		padding: 0;
		display: block;
	}

	.card-icon {
		width: 60px;
		height: 60px;
		margin-bottom: var(--spacing-md);
		position: relative;
	}

	.icon-pulse {
		width: 100%;
		height: 100%;
		background: linear-gradient(135deg, var(--color-cyan), var(--color-cyan-bright));
		border-radius: var(--border-radius-sm);
		animation: icon-rotate 8s linear infinite;
	}

	@keyframes icon-rotate {
		0% {
			transform: rotate(0deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}

	.card-title {
		font-size: var(--font-size-xl);
		font-weight: 600;
		margin-bottom: var(--spacing-sm);
		color: var(--color-text-primary);
	}

	.card-description {
		font-size: var(--font-size-base);
		color: var(--color-text-secondary);
		line-height: 1.6;
	}

	.card-expand-hint {
		position: absolute;
		top: var(--spacing-md);
		right: var(--spacing-md);
		width: 32px;
		height: 32px;
		display: flex;
		align-items: center;
		justify-content: center;
		background: var(--color-cyan);
		color: var(--color-slate);
		border-radius: 50%;
		font-size: var(--font-size-xl);
		font-weight: 700;
		transition: all var(--transition-fast);
	}

	.system-card:hover .card-expand-hint {
		transform: scale(1.1);
	}

	.card-back {
		margin-top: var(--spacing-lg);
		padding-top: var(--spacing-lg);
		border-top: 1px solid var(--glass-border);
	}

	.card-details {
		font-size: var(--font-size-base);
		color: var(--color-text-secondary);
		margin-bottom: var(--spacing-md);
		line-height: 1.8;
	}

	.code-snippet {
		background: rgba(15, 23, 42, 0.8);
		border: 1px solid var(--glass-border);
		border-radius: var(--border-radius-sm);
		padding: var(--spacing-md);
		overflow-x: auto;
	}

	.code-snippet pre {
		margin: 0;
	}

	.code-snippet code {
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
		font-size: var(--font-size-sm);
		color: var(--color-cyan-bright);
		line-height: 1.6;
	}

	.resume-container {
		max-width: 800px;
		margin: 0 auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: var(--spacing-xl);
	}

	.resume-preview {
		width: 100%;
		aspect-ratio: 8.5 / 11;
		background: var(--glass-bg);
		border: 2px solid var(--glass-border);
		border-radius: var(--border-radius-lg);
		backdrop-filter: blur(10px);
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		overflow: hidden;
	}

	.resume-preview::before {
		content: '';
		position: absolute;
		inset: 0;
		background: linear-gradient(135deg, transparent, rgba(6, 182, 212, 0.05));
		pointer-events: none;
	}

	.resume-placeholder {
		text-align: center;
		color: var(--color-text-secondary);
	}

	.resume-icon {
		width: 80px;
		height: 80px;
		margin: 0 auto var(--spacing-md);
		color: var(--color-cyan);
	}

	.download-btn {
		display: inline-flex;
		align-items: center;
		gap: var(--spacing-sm);
		padding: var(--spacing-md) var(--spacing-xl);
		background: linear-gradient(135deg, var(--color-cyan), var(--color-cyan-bright));
		color: var(--color-slate);
		font-size: var(--font-size-base);
		font-weight: 600;
		border-radius: var(--border-radius-sm);
		text-decoration: none;
		position: relative;
		overflow: hidden;
		transition: all var(--transition-base);
	}

	.download-btn::before {
		content: '';
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
		transition: left var(--transition-slow);
	}

	.download-btn:hover::before {
		left: 100%;
	}

	.download-btn:hover {
		transform: translateY(-2px);
		box-shadow: var(--shadow-glow-intense);
	}

	.btn-icon {
		width: 20px;
		height: 20px;
	}

	.terminal {
		max-width: 800px;
		margin: 0 auto;
		background: rgba(15, 23, 42, 0.9);
		border: 1px solid var(--glass-border);
		border-radius: var(--border-radius-md);
		overflow: hidden;
		box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
	}

	.terminal-header {
		display: flex;
		align-items: center;
		gap: var(--spacing-md);
		padding: var(--spacing-sm) var(--spacing-md);
		background: rgba(30, 41, 59, 0.8);
		border-bottom: 1px solid var(--glass-border);
	}

	.terminal-controls {
		display: flex;
		gap: 0.5rem;
	}

	.control {
		width: 12px;
		height: 12px;
		border-radius: 50%;
	}

	.control-close {
		background: #ff5f56;
	}

	.control-minimize {
		background: #ffbd2e;
	}

	.control-maximize {
		background: #27c93f;
	}

	.terminal-title {
		font-size: var(--font-size-sm);
		color: var(--color-text-secondary);
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
	}

	.terminal-body {
		padding: var(--spacing-xl);
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
		font-size: var(--font-size-base);
	}

	.terminal-line {
		color: var(--color-cyan-bright);
		margin-bottom: var(--spacing-sm);
		opacity: 0;
		animation: terminal-fade-in 0.5s ease forwards;
	}

	@keyframes terminal-fade-in {
		to {
			opacity: 1;
		}
	}

	.terminal-line.delay-1 {
		animation-delay: 0.3s;
	}

	.terminal-line.delay-2 {
		animation-delay: 0.6s;
	}

	.terminal-line.delay-3 {
		animation-delay: 0.9s;
	}

	.terminal-line.delay-4 {
		animation-delay: 1.2s;
	}

	.terminal-line.delay-5 {
		animation-delay: 1.5s;
	}

	.terminal-prompt {
		color: var(--color-cyan);
		animation: terminal-fade-in 0.5s ease forwards, cursor-blink 1s step-end infinite;
	}

	@keyframes cursor-blink {
		0%, 50% {
			opacity: 1;
		}
		51%, 100% {
			opacity: 0;
		}
	}

	.footer {
		text-align: center;
		padding: var(--spacing-xl);
		color: var(--color-text-muted);
		font-size: var(--font-size-sm);
		border-top: 1px solid var(--glass-border);
	}

	.footer-year {
		margin-top: var(--spacing-xs);
	}

	@media (max-width: 768px) {
		.main-nav {
			padding: var(--spacing-sm) var(--spacing-md);
		}

		.nav-content {
			flex-direction: column;
			gap: var(--spacing-md);
		}

		.nav-links {
			flex-wrap: wrap;
			justify-content: center;
			gap: var(--spacing-sm);
		}

		.hero {
			padding: var(--spacing-2xl) var(--spacing-md);
		}

		.hero-photo {
			width: 150px;
			height: 150px;
		}

		.photo-container {
			width: 150px;
			height: 150px;
		}

		.systems-grid {
			grid-template-columns: 1fr;
		}

		.systems-section,
		.resume-section,
		.contact-section {
			padding: var(--spacing-2xl) var(--spacing-md);
		}

		.terminal-body {
			padding: var(--spacing-md);
			font-size: var(--font-size-sm);
		}
	}
</style>
