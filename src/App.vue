<template>
  <div class="app-root font-sans" :data-theme="theme">
    <header ref="headerEl" :class="['site-header fixed-top w-100 py-3 py-lg-3', { scrolled: isScrolled }]">
      <div class="container-fluid px-3 px-lg-4 d-flex align-items-center justify-content-between">
        <div class="brand-mark h3 mb-0 d-flex align-items-center will-reveal reveal-left">
          <span>A.</span>&nbsp;<span>S.</span>
        </div>
        <div class="d-flex align-items-center gap-2">
          <button class="theme-toggle me-2" @click="toggleTheme" :aria-label="'Basculer en thème ' + (theme==='dark' ? 'clair' : 'sombre')">
            <i :class="theme==='dark' ? 'bi bi-sun' : 'bi bi-moon-stars'" />
          </button>
          <button class="nav-toggle d-lg-none" :class="{ open: navOpen }" @click="navOpen = !navOpen" aria-label="Menu principal">
            <span></span><span></span><span></span>
          </button>
        </div>
        <div class="nav-wrapper d-lg-flex align-items-center" :class="{ open: navOpen }">
          <nav class="nav-primary mx-lg-3 will-reveal reveal-right">
            <button v-for="item in navItems" :key="item.key" @click="go(item.key)" :class="{ active: currentPage===item.key }" :aria-current="currentPage===item.key ? 'page' : null">{{ item.label }}</button>
          </nav>
          <div class="d-flex align-items-center gap-2 ms-lg-3 mt-3 mt-lg-0 will-reveal reveal-scale">
            <button class="btn btn-outline-accent d-none d-md-inline-flex px-3" @click="go('projects')">Projets</button>
            <button class="btn btn-neon-green rounded-pill px-4 py-2" @click="go('contact')">Discutons</button>
          </div>
        </div>
      </div>
    </header>

    <main class="main-content-area">
      <transition name="view-transition" mode="out-in">
        <component :is="activeComponent" :key="currentPage" :set-current-page="setCurrentPage" @message-submitted="handleNewMessage" />
      </transition>
    </main>

    <footer class="site-footer mt-auto will-reveal">
      <div class="footer-inner container text-center">
        <div class="d-flex flex-column flex-md-row w-100 justify-content-between align-items-center gap-3">
          <p class="mb-0 small text-uppercase letter-spacing-1 muted">&copy; {{ new Date().getFullYear() }} Abdoulhalim SOILIHI • Freelance Java / Spring</p>
          <VisitCounter />
          <div class="socials">
            <a href="mailto:soilihi.abdoulhalim@outlook.fr" aria-label="Email"><i class="bi bi-envelope"></i></a>
            <a href="https://www.linkedin.com/in/abdoulhalim-soilihi-b749779b" target="_blank" rel="noopener" aria-label="LinkedIn"><i class="bi bi-linkedin"></i></a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import axios from 'axios';
import VisitCounter from './components/VisitCounter.vue';
import profilePic from './assets/profil_pic.jpeg';

const currentPage = ref('home');
const navOpen = ref(false);
const isScrolled = ref(false);
const theme = ref(localStorage.getItem('theme-mode') || (window.matchMedia('(prefers-color-scheme: light)').matches ? 'light' : 'dark'));

const navItems = [
  { key: 'home', label: 'Accueil' },
  { key: 'skills', label: 'Services' },
  { key: 'projects', label: 'Projets' },
  { key: 'about', label: 'À propos' },
  { key: 'contact', label: 'Contact' }
];

const setCurrentPage = (page) => { currentPage.value = page; };
const go = (page) => { currentPage.value = page; navOpen.value = false; window.scrollTo({ top:0, behavior:'smooth'}); };

const handleScroll = () => { isScrolled.value = window.scrollY > 8; };
window.addEventListener('scroll', handleScroll, { passive: true });
handleScroll();

const toggleTheme = () => {
  theme.value = theme.value === 'dark' ? 'light' : 'dark';
  localStorage.setItem('theme-mode', theme.value);
  document.documentElement.setAttribute('data-theme', theme.value);
};
document.documentElement.setAttribute('data-theme', theme.value);

// --- Composants de Page ---
const HomePage = {
  props: ['setCurrentPage'],
  setup(props){
    const goContact = () => props.setCurrentPage && props.setCurrentPage('contact');
    const goProjects = () => props.setCurrentPage && props.setCurrentPage('projects');
    return { goContact, goProjects, profilePic };
  },
  template: `
    <section class="container hero">
      <div class="d-flex flex-column justify-content-center" style="z-index:2;">
        <h1 class="hero-title anim-soft anim-d1">Développeur <span class="accent-text">Java / Spring</span> Freelance</h1>
        <p class="hero-sub anim-soft anim-d2">Je conçois des backends robustes, scalables et maintenables (Java 8→21, Spring Boot, microservices, Kafka, CI/CD, Cloud). Mon objectif: accélérer vos produits tout en garantissant qualité et fiabilité.</p>
        <div class="d-flex flex-wrap gap-2 mb-3 anim-soft anim-d3">
          <button class="btn btn-neon-green rounded-pill px-4 py-2 me-2 mb-2" @click="goContact">Me contacter</button>
          <button class="btn btn-outline-accent rounded-pill px-4 py-2 mb-2" @click="goProjects">Voir projets</button>
        </div>
        <div class="stats-grid anim-soft anim-d4">
          <div class="stat-item anim-soft anim-d2"><h3>9+</h3><p>Années</p></div>
          <div class="stat-item anim-soft anim-d3"><h3>8+</h3><p>Projets clés</p></div>
          <div class="stat-item anim-soft anim-d4"><h3>100%</h3><p>Satisfaction</p></div>
        </div>
      </div>
      <div class="position-relative d-flex align-items-center justify-content-center anim-soft anim-d3">
        <div class="glass-card p-0 overflow-hidden profile-frame" style="max-width:320px;aspect-ratio:3/4;">
          <img :src="profilePic" alt="Photo profil" class="w-100 h-100" style="object-fit:cover;" />
        </div>
      </div>
    </section>
  `
};

const SkillsPage = {
  setup(){
    const skillCategories = ref([
      { title: 'Core Java & Frameworks', items: ['Java (8-21)', 'Spring Boot / Framework', 'Hibernate · JPA', 'APIs REST / SOAP / OpenAPI'] },
      { title: 'DevOps & Cloud', items: ['Docker', 'Kubernetes (GKE)', 'CI/CD GitLab · Jenkins · GitHub', 'Observabilité / Monitoring'] },
      { title: 'Datastores & Messaging', items: ['PostgreSQL · Oracle', 'MongoDB · Redis', 'Kafka (Conduktor)', 'Optimisation requêtes'] },
      { title: 'Qualité & Tests', items: ['JUnit · Mockito', 'TDD / Clean Code', 'SonarQube', 'Perf & Profiling'] },
      { title: 'Frontend (Notions)', items: ['Vue.js', 'AngularJS', 'JavaScript ES', 'HTML5 · CSS3 · Bootstrap'] },
      { title: 'Outils & Méthodo', items: ['IntelliJ · Eclipse', 'Git / GitHub / GitLab / SVN', 'Jira · Confluence', 'Agile Scrum / Kanban'] }
    ]);
    return { skillCategories };
  },
  template: `
    <section class="section-skills py-5">
      <div class="container">
        <h2 class="section-title text-center anim-soft anim-d1">Compétences</h2>
        <div class="row g-4 anim-soft anim-d2">
          <div v-for="(cat,i) in skillCategories" :key="i" class="col-12 col-md-6 col-xl-4 anim-soft anim-d3">
            <div class="glass-card card-surface skill-category h-100">
              <h3>{{ cat.title }}</h3>
              <ul class="skill-list">
                <li v-for="(item,j) in cat.items" :key="j"><span class="skill-bullet"></span><span>{{ item }}</span></li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </section>
  `
};

const ProjectsPage = {
  setup(){
    const projects = ref([
      {
        title: 'Gestion des Commandes & Logistique (Decathlon)',
        client: 'Decathlon',
        period: 'Nov 2023 - En cours',
        tags:['Java 21','Spring Boot','Kafka','GKE'],
        description: 'Développement backend Java (Java 21, Spring Boot, Kafka, GKE) pour un écosystème de trois applications stratégiques (Shipperbox, Relay Point Manager, Trackbox) gérant les commandes et la logistique.',
        details: 'Contribution à la stabilité, la scalabilité et la performance, en respectant les pratiques DevOps et les normes de sécurité.'
      },
      {
        title: 'Interfaces B2B & écosystème Data/Event Driven (Experis/Adeo)',
        client: 'Experis/Adeo',
        period: '9 mois',
        tags:['Java 17','Event Driven','Docker'],
        description: 'Développement d\'interfaces entre un backbone transactionnel B2B et un écosystème data/event driven (Java 17, Spring Boot 3).',
        details: 'Mise en place de services Docker Java, déploiements, gestion de features, revues de code, documentation, tests unitaires (JUnit), création de topics Kafka et suivi du RUN (Data Dog).'
      },
      {
        title: 'Optimisation Cloud & Infrastructure (Sogeti/Leroy Merlin)',
        client: 'Sogeti/Leroy Merlin',
        period: '11 mois',
        tags:['GKE','Monitoring','Vue.js'],
        description: 'Initialisation de nouveaux composants Java Spring Boot, déploiement sur Google Kubernetes Engine (GKE), maintenance et évolutions.',
        details: 'Participation aux bascules de nouveaux magasins. Prise en charge de modules Front (Vue.js) et utilisation d\'outils de monitoring (Data Dog).'
      },
      {
        title: 'Développeur Agile / Capgemini - GRDF',
        client: 'GRDF',
        period: '2 ans 5 mois',
        tags:['Java EE','Maintenance','Refactoring'],
        description: 'Amélioration et maintenance des portails Fournisseurs et Distributeur pour le client GRDF.',
        details: 'Application permettant le suivi et la facturation de la distribution du GAZ (Backend).'
      },
      {
        title: 'API Bancaire Sécurisée & Gestion de Comptes (BNP Paribas Cardif)',
        client: 'BNP Paribas Cardif',
        period: '1 an 10 mois',
        tags:['OAuth2','Security','Swagger'],
        description: 'Architecture et implémentation d\'APIs hautement sécurisées pour les virements et la gestion de comptes.',
        details: 'Mise en place d\'OAuth2, tests rigoureux avec JUnit et Postman, et documentation complète via Swagger. Travail sur la création et la gestion d\'applications de gestion des fonds.'
      },
      {
        title: 'Ingénieur réalisateur - Société générale',
        client: 'Société générale',
        period: '7 mois',
        tags:['Java','PostgreSQL','Maintenance'],
        description: 'Maintenance et évolution du poste de travail : Outil de la société générale, utilisé par les conseillers du crédit du Nord pour la création des produits bancaires (Comptes, Cartes, etc.).',
        details: 'Équipe d\'une dizaine de personnes. Maintenance et évolution d\'applications critiques pour les conseillers bancaires.'
      },
      {
        title: 'Ingénieur Réalisateur (Java EE 1.8, Tomcat, PostgreSQL) - Ministère de la transition écologique',
        client: 'Ministère de la transition écologique',
        period: '1 an 3 mois',
        tags:['Java EE','PostgreSQL','Secteur Public'],
        description: 'Projet du ministère de la transition écologique (IED : Industrial Emissions Directive).',
        details: 'Outil permettant de constituer et de suivre les dossiers des éleveurs. Développement d\'une application de conformité environnementale.'
      }
    ]);
    return { projects };
  },
  template: `
    <section class="section-projects py-5">
      <div class="container">
        <h2 class="section-title text-center anim-soft anim-d1">Projets</h2>
        <div class="row g-4 anim-soft anim-d2">
          <div v-for="(p,i) in projects" :key="i" class="col-12 col-md-6 col-xl-4 anim-soft anim-d3">
            <div class="glass-card card-surface project-category h-100">
              <div class="d-flex justify-content-between align-items-start mb-2">
                <h3 class="mb-0">{{ p.title }}</h3>
                <span class="project-period text-muted small">{{ p.period }}</span>
              </div>
              <p class="project-client small mb-2">{{ p.client }}</p>
              <p class="project-desc mb-2">{{ p.description }}</p>
              <p class="project-details mb-3 flex-grow-1">{{ p.details }}</p>
              <div class="tag-badges mt-auto pt-1">
                <span v-for="(t,j) in p.tags" :key="j" class="tag" :class="{ 'tag-accent': j===0 }">{{ t }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  `
};

const AboutMePage = {
  template: `
    <section class="section-about py-5">
      <div class="container">
        <h2 class="section-title text-center anim-soft anim-d1">À propos</h2>
        <div class="row justify-content-center anim-soft anim-d2 g-4">
          <div class="col-12 col-lg-10 col-xl-8">
            <div class="glass-card card-surface about-category">
              <p class="lead-paragraph mb-3">Je suis <strong>Abdoulhalim</strong>, développeur backend spécialisé Java / Spring, focalisé sur la qualité, la performance et l'évolution maîtrisée des systèmes.</p>
              <p class="lead-paragraph mb-3">J'aide les équipes à livrer plus rapidement grâce à une architecture claire, des pipelines CI/CD fiables, une observabilité intégrée et une culture de code propre.</p>
              <p class="lead-paragraph mb-0">Valeurs: simplicité pragmatique, transmission, esprit d'équipe, amélioration continue.</p>
            </div>
          </div>
        </div>
      </div>
    </section>
  `
};

const ContactPage = {
  emits: ['message-submitted'],
  setup(_, { emit }) {
    const name = ref('');
    const email = ref('');
    const message = ref('');
    const status = ref('');
    const sending = ref(false);
    const handleSubmit = async (e) => {
      e.preventDefault();
      if(sending.value) return;
      sending.value = true;
      status.value = 'Envoi en cours...';
      try {
        const URL_BACKEND_API = 'https://yammering-saloma-lhabdou-7d769c63.koyeb.app/api/send-email';
        const response = await axios.post(URL_BACKEND_API, { name: name.value, email: email.value, message: message.value });
        if (response.status === 200) {
          status.value = 'Message envoyé avec succès !';
          emit('message-submitted', { name: name.value, email: email.value });
          name.value=''; email.value=''; message.value='';
        } else {
          status.value = "Échec de l'envoi. Réessayez.";
        }
      } catch (err) {
        console.error(err);
        status.value = "Erreur lors de l'envoi.";
      } finally {
        sending.value = false;
        setTimeout(()=> status.value='', 6000);
      }
    };
    return { name, email, message, status, handleSubmit, sending };
  },
  template: `
    <section class="section-contact py-5">
      <div class="container d-flex flex-column align-items-center">
        <h2 class="section-title text-center anim-soft anim-d1">Contact</h2>
        <form @submit.prevent="handleSubmit" class="glass-card w-100 anim-soft anim-d2" style="max-width:700px;">
          <div class="row g-4 p-4 p-md-5">
            <div class="col-12 col-md-6">
              <label for="name" class="form-label">Nom</label>
              <input id="name" type="text" v-model="name" required class="form-control" placeholder="Votre nom" />
            </div>
            <div class="col-12 col-md-6">
              <label for="email" class="form-label">Email</label>
              <input id="email" type="email" v-model="email" required class="form-control" placeholder="vous@exemple.com" />
            </div>
            <div class="col-12">
              <label for="message" class="form-label">Message</label>
              <textarea id="message" rows="6" v-model="message" required class="form-control" placeholder="Décrivez votre besoin..."></textarea>
            </div>
            <div class="col-12">
              <button type="submit" class="btn btn-neon-green w-100 py-3" :disabled="sending">{{ sending ? 'Envoi...' : 'Envoyer le message' }}</button>
            </div>
            <div class="col-12" v-if="status">
              <p class="text-center text-neon-green mb-0 small">{{ status }}</p>
            </div>
          </div>
        </form>
        <div class="mt-4 text-center muted small anim-soft anim-d3">
          Ou écrivez directement à <a href="mailto:soilihi.abdoulhalim@outlook.fr" class="text-gradient text-decoration-none">soilihi.abdoulhalim@outlook.fr</a>
        </div>
      </div>
    </section>
  `
};

const pagesMap = { home: HomePage, skills: SkillsPage, projects: ProjectsPage, about: AboutMePage, contact: ContactPage };
const activeComponent = computed(() => pagesMap[currentPage.value] || HomePage);

const handleNewMessage = (payload) => { console.log('Nouveau message', payload); };
</script>

<style scoped>
.letter-spacing-1 { letter-spacing: .12em; }
.accent-text { color: var(--neon-green); }
.project-details { color:#d6e1eb; font-size:.9rem; line-height:1.5; }

/* Force tous les fonds de cartes en sombre, peu importe le thème */
.glass-card {
  background: rgba(15, 23, 31, 0.8) !important;
  backdrop-filter: blur(10px) !important;
  border: 1px solid rgba(64, 224, 208, 0.2) !important;
  color: #ffffff !important;
}

.card-surface {
  background: rgba(15, 23, 31, 0.9) !important;
  color: #ffffff !important;
}

/* Force le texte des cartes en clair */
.glass-card h3,
.glass-card p,
.glass-card .project-desc,
.glass-card .project-details {
  color: #ffffff !important;
}

.glass-card .project-details {
  color: #d6e1eb !important;
}

/* Amélioration de la lisibilité des projets - style similaire aux compétences */
.project-desc {
  font-size: 0.95rem !important;
  line-height: 1.6 !important;
  color: #e2e8f0 !important;
  margin-bottom: 0.75rem !important;
}

/* Style des compétences pour référence et cohérence */
.skill-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.skill-list li {
  padding: 0.4rem 0;
  font-size: 0.9rem;
  line-height: 1.6;
  color: #e2e8f0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.skill-bullet {
  width: 4px;
  height: 4px;
  background: var(--neon-green, #40e0d0);
  border-radius: 50%;
  flex-shrink: 0;
}

/* Styles pour les nouveaux champs des projets */
.project-period {
  font-size: 0.8rem !important;
  color: var(--neon-green, #40e0d0) !important;
  font-weight: 500 !important;
  background: rgba(64, 224, 208, 0.1) !important;
  padding: 0.2rem 0.5rem !important;
  border-radius: 12px !important;
  white-space: nowrap;
}

.project-client {
  font-size: 1.0rem !important;
  color: var(--neon-green, #40e0d0) !important;
  font-style: normal !important;
  font-weight: 700 !important;
  margin-bottom: 0.5rem !important;
  text-transform: uppercase !important;
  letter-spacing: 0.05em !important;
  background: rgba(64, 224, 208, 0.15) !important;
  padding: 0.3rem 0.6rem !important;
  border-radius: 8px !important;
  display: inline-block !important;
}

/* Réduction de la taille des titres de projets */
.glass-card h3 {
  font-size: 1.1rem !important;
  font-weight: 600 !important;
  line-height: 1.3 !important;
}
</style>
