<script>
  import { fade, fly, scale } from 'svelte/transition';
  import { cubicOut, elasticOut } from 'svelte/easing';
  import Hero from '$lib/components/Hero.svelte';
  import LanguageSwitcher from '$lib/components/LanguageSwitcher.svelte';
  import SystemsArchitecture from '$lib/components/SystemsArchitecture.svelte';
  import Resume from '$lib/components/Resume.svelte';
  import Contact from '$lib/components/Contact.svelte';
  import Navigation from '$lib/components/Navigation.svelte';

  /** @type {'en' | 'ka' | 'ru'} */
  let currentLang = $state('en');

  /**
   * Translations object for multi-language support
   * @type {Record<string, Record<string, string>>}
   */
  const translations = {
    en: {
      heroTitle: 'Engineering Demand-Driven Automation',
      heroSubtitle: "I don't just write code; I build future-proof logic.",
      systemsTitle: 'Systems Architecture & Logic',
      resumeTitle: 'Professional Experience',
      contactTitle: 'Connect',
      downloadCV: 'Download CV',
      name: 'Name',
      email: 'Email',
      message: 'Message',
      send: 'Transmit Signal',
      card1Title: 'Adaptive AI Pipelines',
      card1Desc: 'Logic for autonomous workflow scaling',
      card2Title: 'Future-Proof Architectures',
      card2Desc: 'Designing systems that anticipate automation',
      card3Title: 'Demand-Driven Development',
      card3Desc: 'Creating solutions based on necessity, not just standards'
    },
    ka: {
      heroTitle: 'მოთხოვნაზე ორიენტირებული ავტომატიზაცია',
      heroSubtitle: 'მე არ ვწერ უბრალოდ კოდს; ვაშენებ მომავლისთვის მზა ლოგიკას.',
      systemsTitle: 'სისტემების არქიტექტურა და ლოგიკა',
      resumeTitle: 'პროფესიული გამოცდილება',
      contactTitle: 'კონტაქტი',
      downloadCV: 'CV-ის ჩამოტვირთვა',
      name: 'სახელი',
      email: 'ელ-ფოსტა',
      message: 'შეტყობინება',
      send: 'სიგნალის გაგზავნა',
      card1Title: 'ადაპტური AI მილსადენები',
      card1Desc: 'ავტონომიური სამუშაო პროცესების მასშტაბირების ლოგიკა',
      card2Title: 'მომავლისთვის მზა არქიტექტურები',
      card2Desc: 'სისტემების დიზაინი, რომლებიც ითვალისწინებენ ავტომატიზაციას',
      card3Title: 'მოთხოვნაზე ორიენტირებული განვითარება',
      card3Desc: 'გადაწყვეტილებების შექმნა საჭიროების მიხედვით'
    },
    ru: {
      heroTitle: 'Автоматизация на основе спроса',
      heroSubtitle: 'Я не просто пишу код; я создаю логику будущего.',
      systemsTitle: 'Системная архитектура и логика',
      resumeTitle: 'Профессиональный опыт',
      contactTitle: 'Связаться',
      downloadCV: 'Скачать CV',
      name: 'Имя',
      email: 'Эл. почта',
      message: 'Сообщение',
      send: 'Отправить сигнал',
      card1Title: 'Адаптивные AI пайплайны',
      card1Desc: 'Логика для автономного масштабирования рабочих процессов',
      card2Title: 'Архитектуры будущего',
      card2Desc: 'Проектирование систем с учётом автоматизации',
      card3Title: 'Разработка по требованию',
      card3Desc: 'Создание решений на основе необходимости'
    }
  };

  /**
   * Get translation for current language
   * @param {string} key
   */
  function t(key) {
    return translations[currentLang][key] || key;
  }
</script>

<svelte:head>
  <title>Giorgi Tchanturia | Full Stack Developer & AI Engineer</title>
  <meta name="description" content="Portfolio of Giorgi Tchanturia - Engineering demand-driven automation and future-proof systems." />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
</svelte:head>

<div class="portfolio">
  <Navigation {currentLang} {t} />
  
  <LanguageSwitcher bind:currentLang />
  
  <main>
    <Hero {t} />
    <SystemsArchitecture {t} />
    <Resume {t} />
    <Contact {t} />
  </main>
  
  <footer class="footer">
    <div class="footer-content">
      <p class="footer-text">
        <span class="footer-symbol">&copy;</span> 2024 Giorgi Tchanturia
        <span class="footer-divider">|</span>
        <span class="footer-tagline">Built with precision and purpose</span>
      </p>
    </div>
  </footer>
</div>

<style>
  /* ==========================================================================
     Main Portfolio Container
     ========================================================================== */
  .portfolio {
    position: relative;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }

  main {
    flex: 1;
  }

  /* ==========================================================================
     Footer
     ========================================================================== */
  .footer {
    padding: var(--space-8) var(--space-6);
    border-top: 1px solid var(--glass-border);
    background: var(--glass-bg);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
  }

  .footer-content {
    max-width: 1200px;
    margin: 0 auto;
    text-align: center;
  }

  .footer-text {
    font-size: var(--text-sm);
    color: var(--color-slate-400);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: var(--space-2);
    flex-wrap: wrap;
  }

  .footer-symbol {
    color: var(--color-cyan-400);
  }

  .footer-divider {
    color: var(--color-slate-600);
  }

  .footer-tagline {
    color: var(--color-slate-500);
  }
</style>
