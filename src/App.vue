<template>
  <div class="bg-dark text-white font-sans min-vh-100">
    <header class="fixed-top bg-dark text-white py-4 px-3 shadow-lg d-flex justify-content-between align-items-center">
      <div class="h3 font-weight-bold text-neon-green mb-0 animate-fade-in-down">A.S.</div>
      <nav class="d-flex flex-wrap justify-content-center animate-fade-in-down animation-delay-200">
        <button @click="currentPage = 'home'"
          :class="['nav-link', { 'active': currentPage === 'home' }]">ACCUEIL</button>
        <button @click="currentPage = 'skills'"
          :class="['nav-link', { 'active': currentPage === 'skills' }]">SERVICES</button>
        <button @click="currentPage = 'projects'"
          :class="['nav-link', { 'active': currentPage === 'projects' }]">PROJETS</button>
        <button @click="currentPage = 'about'" :class="['nav-link', { 'active': currentPage === 'about' }]">À
          PROPOS</button>
        <button @click="currentPage = 'contact'"
          :class="['nav-link', { 'active': currentPage === 'contact' }]">CONTACT</button>
      </nav>
      <button @click="currentPage = 'contact'"
        class="btn btn-neon-green text-dark font-weight-bold py-2 px-4 rounded-pill shadow-sm animate-fade-in-down animation-delay-400 d-none d-md-block">
        Discutons
      </button>
    </header>

    <main class="main-content-area">
      <div v-if="currentPage === 'home'">
        <HomePage :set-current-page="setCurrentPage" />
      </div>
      <div v-else-if="currentPage === 'skills'">
        <SkillsPage />
      </div>
      <div v-else-if="currentPage === 'projects'">
        <ProjectsPage />
      </div>
      <div v-else-if="currentPage === 'about'">
        <AboutMePage />
      </div>
      <div v-else-if="currentPage === 'contact'">
        <ContactPage @message-submitted="handleNewMessage" />
      </div>
    </main>

    <div v-if="submittedMessages.length > 0" style="display: none;">
      <h2>Messages Reçus via le Formulaire</h2>
      <ul>
        <li v-for="(msg, index) in submittedMessages" :key="index">
          <p><strong>Date:</strong> {{ msg.timestamp }}</p>
          <p><strong>Nom:</strong> {{ msg.name }}</p>
          <p><strong>Email:</strong> {{ msg.email }}</p>
          <p><strong>Message:</strong> {{ msg.message }}</p>
          <hr />
        </li>
      </ul>
    </div>


    <footer class="bg-dark text-secondary text-center py-4 border-top border-secondary border-opacity-25 mt-auto">
      &copy; {{ new Date().getFullYear() }} Abdoulhalim SOILIHI - Freelance Java. Tous droits réservés.
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// État pour gérer la page actuelle
const currentPage = ref('home');

// NOUVEAU : Liste réactive pour stocker les messages
const submittedMessages = ref([]);

// Fonction pour changer la page actuelle
const setCurrentPage = (page) => {
  currentPage.value = page;
};

// NOUVEAU : Fonction pour ajouter un message à notre liste
const handleNewMessage = (messageData) => {
  const messageWithTimestamp = {
    ...messageData,
    timestamp: new Date().toISOString() // Ajoute un horodatage
  };
  submittedMessages.value.push(messageWithTimestamp);
  console.log("Liste des messages sauvegardés :", submittedMessages.value);
};


// --- Composants de Page ---

const HomePage = {
  props: ['setCurrentPage'],
  template: `
    <div class="d-flex flex-column flex-lg-row min-vh-100 bg-dark text-white py-5 px-3 px-lg-5 position-relative">
      <div class="flex-grow-1 d-flex flex-column justify-content-center align-items-start z-1 py-5 py-lg-0">
        <h2 class="display-3 display-lg-1 font-weight-bolder mb-4 animate-fade-in-left">Abdoulhalim SOILIHI</h2>
        <div class="position-relative d-inline-block mb-4 animate-fade-in-left animation-delay-300">
          <span class="developer-badge text-neon-green text-uppercase font-weight-bold border border-neon-green px-4 py-2 d-inline-block">DEVELOPPEUR JAVA/SPRING</span>
        </div>
        <button @click="setCurrentPage('contact')" class="btn btn-link text-white text-lg border-bottom border-transparent hover-border-neon-green transition-all duration-300 d-flex align-items-center group mb-5 animate-fade-in-left animation-delay-600">
          Contactez-moi <span class="ms-2 group-hover-translate-x-2 transition-transform duration-300">→</span>
        </button>
        <div class="row w-100 text-center animate-fade-in-up animation-delay-900">
          <div class="col-12 col-sm-4 mb-4 mb-sm-0"><p class="h1 font-weight-bold text-neon-green">9+</p><p class="text-secondary text-lg">Ans d'expérience</p></div>
          <div class="col-12 col-sm-4 mb-4 mb-sm-0"><p class="h1 font-weight-bold text-neon-green">8+</p><p class="text-secondary text-lg">Projets réussis</p></div>
          <div class="col-12 col-sm-4"><p class="h1 font-weight-bold text-neon-green">100%</p><p class="text-secondary text-lg">Satisfaction client</p></div>
        </div>
      </div>
      <div class="position-relative w-100 w-lg-40 d-flex align-items-center justify-content-center justify-content-lg-end z-1 mt-5 mt-lg-0 animate-fade-in-right">
        <div class="position-relative w-100 h-300px h-sm-400px h-lg-100 w-lg-400px bg-neon-green d-flex align-items-end justify-content-center rounded shadow-lg overflow-hidden">
          <div class="position-absolute inset-0 bg-secondary bg-cover bg-center rounded" style="background-image: url('https://via.placeholder.com/600x800?text=Abdoulhalim+SOILIHI');"></div>
          <div class="position-absolute top-50 end-0 transform-translate-y-50 translate-x-50 rotate-90 origin-bottom-left text-dark text-2xl font-weight-bold text-nowrap d-none d-lg-block">MON EXPERTISE</div>
        </div>
      </div>
    </div>
  `,
};

const SkillsPage = {
  template: `
    <div class="min-vh-100 bg-dark text-white py-5 px-3 px-lg-5">
      <h2 class="display-4 font-weight-bolder text-neon-green mb-5 text-center animate-fade-in">Mes Compétences Techniques</h2>
      <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
        <div v-for="(skillCategory, index) in skillCategories" :key="index" class="col">
          <div class="card bg-dark-card border border-secondary border-opacity-25 h-100 shadow-sm hover-border-neon-green transition-all duration-300 animate-fade-in-up" :style="{ animationDelay: index * 100 + 'ms' }">
            <div class="card-body">
              <h3 class="card-title h4 font-weight-bold text-neon-green mb-4">{{ skillCategory.title }}</h3>
              <ul class="list-unstyled text-secondary space-y-2 text-lg">
                <li v-for="(item, i) in skillCategory.items" :key="i" class="d-flex align-items-center">
                    <svg class="me-2 text-neon-green" width="1em" height="1em" viewBox="0 0 16 16" fill="currentColor" xmlns="http://www.w3.org/2000/svg"><path d="M10.97 4.97a.75.75 0 0 1 1.071 1.05l-3.992 4.99a.75.75 0 0 1-1.08.02L4.324 8.384a.75.75 0 1 1 1.06-1.06l2.094 2.093 3.473-4.425a.235.235 0 0 1 .02-.022z"/></svg>
                    {{ item }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
  setup() {
    const skillCategories = ref([
      { title: 'Core Java & Frameworks', items: ['Java (8-21) / Spring Boot', 'Spring Framework / Batch / Security', 'Hibernate / JPA', 'APIs REST / SOAP / OpenAPI'] },
      { title: 'DevOps & Cloud', items: ['Docker / Kubernetes (GKE)', 'CI/CD (GitLab / Jenkins / GitHub)', 'SFTP / SSH'] },
      { title: 'Bases de Données & Messagerie', items: ['PostgreSQL / Oracle / SQL Server', 'MongoDB / Redis', 'Kafka / Conduktor'] },
      { title: 'Tests & Qualité Logicielle', items: ['JUnit / Mockito / DB Unit', 'Sonarqube / TDD', 'Analyse de performance'] },
      { title: 'Frontend (Connaissances)', items: ['Vue.js / Angular JS', 'JSP / JavaScript', 'HTML5 / CSS3 / Bootstrap'] },
      { title: 'Outils & Méthodologies', items: ['IntelliJ IDEA / Eclipse', 'Jira / Confluence / SharePoint', 'Git / GitHub / GitLab / SVN', 'Data Dog / MobaXterm / SoapUI / FileZilla', 'Agile (Scrum / Kanban)'] },
    ]);
    return { skillCategories };
  }
};

const ProjectsPage = {
  template: `
    <div class="min-vh-100 bg-dark text-white py-5 px-3 px-lg-5">
      <h2 class="display-4 font-weight-bolder text-neon-green mb-5 text-center animate-fade-in">Mes Réalisations Clés</h2>
      <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
        <div v-for="(project, index) in projects" :key="index" class="col">
          <div class="card bg-dark-card border border-secondary border-opacity-25 h-100 shadow-sm hover-border-neon-green transition-all duration-300 animate-fade-in-up" :style="{ animationDelay: index * 100 + 'ms' }">
            <div class="card-body">
              <h3 class="card-title h4 font-weight-bold text-neon-green mb-3">{{ project.title }}</h3>
              <p class="card-text text-secondary text-lg" v-html="project.description"></p>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
  setup() {
    const projects = ref([
      { title: 'Gestion des Commandes & Logistique (Decathlon)', description: 'Nov 2023 - En cours: Développement backend Java (Java 21, Spring Boot, Kafka, GKE) pour un écosystème de trois applications stratégiques (Shipperbox, Relay Point Manager, Trackbox) gérant les commandes et la logistique. Contribution à la stabilité, la scalabilité et la performance, en respectant les pratiques DevOps et les normes de sécurité.' },
      { title: 'Interfaces B2B & Écosystème Data/Event Driven (Experis/Adeo)', description: '9 mois: Développement d\'interfaces entre un backbone transactionnel B2B et un écosystème data/event driven (Java 17, Spring Boot 3). Mise en place de services Docker Java, déploiements, gestion de features, revues de code, documentation, tests unitaires (JUnit), création de topics Kafka et suivi du RUN (Data Dog).' },
      { title: 'Optimisation Cloud & Infrastructure (Sogeti/Leroy Merlin)', description: '11 mois: Initialisation de nouveaux composants Java Spring Boot, déploiement sur Google Kubernetes Engine (GKE), maintenance et évolutions. Participation aux bascules de nouveaux magasins. Prise en charge de modules Front (Vue.js) et utilisation d\'outils de monitoring (Data Dog).' },
      { title: 'Développeur Agile / Capgemini - GrDF ', description: '2 ans 5 mois: Amélioration et maintenance des portails Fournisseurs et Distributeur pour le client GRDF. Application permettant le suivi et la facturation de la distribution du GAZ (Backend)' },
      { title: 'API Bancaire Sécurisée & Gestion de Comptes (BNP Paribas Cardif)', description: '1 an 10 mois: Architecture et implémentation d\'APIs hautement sécurisées pour les virements et la gestion de comptes. Mise en place d\'OAuth2, tests rigoureux avec JUnit et Postman, et documentation complète via Swagger. Travail sur la création et la gestion d\'applications de gestion des fonds.' },
      { title: 'Ingénieur réalisateur - Société générale', description: '7 mois: Maintenance et évolution du poste de travail : Outil de la société générale, utilisé par les conseillers du crédit duMaintenance et évolution du poste de travail : Outil de la société générale, utilisé par les conseillers du crédit du Nord pour la création des produits bancaires (Comptes, Cartes, etc.). Equipe d’une dizaine de personnes.' },
      { title: 'Ingénieur Réalisateur (Java EE 1.8, Tomcat, PostgreSql) - Ministère de la transition écologique', description: '1 an 3mois: Projet du ministère de la transition écologique (IED : Industrial Emissions Directive), Outil permettant de constituer et de suivre les dossiers des éleveurs. ' },
      { title: 'Ingénieur réalisateur - Société générale', description: '7 mois: Maintenance et évolution du poste de travail : Outil de la société générale, utilisé par les conseillers du crédit du Nord pour la création des produits bancaires (Comptes, Cartes, etc.). Equipe d’une dizaine de personnes.' },
    ]);
    return { projects };
  }
};

const AboutMePage = {
  template: `
    <div class="min-vh-100 bg-dark text-white py-5 px-3 px-lg-5 d-flex flex-column justify-content-center align-items-center text-center">
      <h2 class="display-4 font-weight-bolder text-neon-green mb-5 animate-fade-in">À Propos de Moi</h2>
      <div class="card bg-dark-card border border-secondary border-opacity-25 shadow-lg p-5 rounded animate-fade-in-up" style="max-width: 900px;">
        <div class="card-body">
          <p class="card-text text-secondary text-lg mb-4">Abdoulhalim, Développeur Java/Spring Freelance passionné par la création de solutions backend robustes, évolutives et performantes. Fort de plus de 9 ans d'expérience dans l'écosystème Java, je me suis spécialisé dans le développement avec <strong>Spring Boot</strong>, l'architecture <strong>microservices</strong>, l'intégration continue (<strong>CI/CD</strong>) et les pratiques <strong>DevOps</strong>.</p>
          <p class="card-text text-secondary text-lg mb-4">Mon objectif est d'aider les entreprises à transformer leurs idées en applications concrètes, en apportant une expertise technique solide et une approche axée sur la qualité et l'innovation. J'aime résoudre des problèmes complexes et construire des systèmes fiables qui répondent aux besoins de mes clients.</p>
          <p class="card-text text-secondary text-lg">Mes valeurs professionnelles fondamentales sont le respect, le partage, un esprit d'équipe solide et la simplification du code, tout en respectant les bonnes pratiques. Je suis constamment à l'affût des nouvelles technologies et des meilleures pratiques pour garantir que les solutions que je livre sont à la pointe de l'industrie.</p>
        </div>
      </div>
    </div>
  `,
};

const ContactPage = {
  // MODIFIÉ : Déclaration de l'événement que le composant peut émettre
  emits: ['message-submitted'],
  template: `
    <div class="min-vh-100 bg-dark text-white py-5 px-3 px-lg-5 d-flex flex-column justify-content-center align-items-center">
      <h2 class="display-4 font-weight-bolder text-neon-green mb-5 text-center animate-fade-in">Entrons en Contact</h2>
      <form @submit.prevent="handleSubmit" class="w-100 card bg-dark-card border border-secondary border-opacity-25 shadow-lg p-5 rounded animate-fade-in-up" style="max-width: 700px;">
        <div class="mb-4">
          <label for="name" class="form-label text-secondary h5 font-weight-bold">Votre Nom</label>
          <input type="text" id="name" v-model="name" class="form-control bg-dark text-white border-secondary border-opacity-25 focus-border-neon-green focus-shadow-neon-green" placeholder="Votre Nom" required />
        </div>
        <div class="mb-4">
          <label for="email" class="form-label text-secondary h5 font-weight-bold">Votre Email</label>
          <input type="email" id="email" v-model="email" class="form-control bg-dark text-white border-secondary border-opacity-25 focus-border-neon-green focus-shadow-neon-green" placeholder="votre.email@exemple.com" required />
        </div>
        <div class="mb-5">
          <label for="message" class="form-label text-secondary h5 font-weight-bold">Votre Message</label>
          <textarea id="message" rows="6" v-model="message" class="form-control bg-dark text-white border-secondary border-opacity-25 focus-border-neon-green focus-shadow-neon-green resize-vertical" placeholder="Décrivez votre projet ou votre question..." required></textarea>
        </div>
        <button type="submit" class="btn btn-neon-green text-dark font-weight-bold py-3 px-5 rounded shadow-lg w-100 text-xl text-uppercase letter-spacing-1">Envoyer le message</button>
        <p v-if="status" class="mt-4 text-center text-neon-green h5 animate-fade-in">{{ status }}</p>
      </form>
      <div class="mt-5 text-center">
        <h4 class="text-neon-green mb-3">Mes Coordonnées</h4>
        <p class="text-secondary text-lg mb-2"><svg class="me-2 text-neon-green" width="1em" height="1em" viewBox="0 0 16 16" fill="currentColor" xmlns="http://www.w3.org/2000/svg"><path d="M.05 3.555A2 2 0 0 1 2 2h12a2 2 0 0 1 1.95 1.555L8 8.414.05 3.555zM0 4.697v7.104l5.803-3.558L0 4.697zM6.761 8.83l-6.57 4.027A2 2 0 0 0 2 14h12a2 2 0 0 0 1.95-1.143l-6.57-4.027L8 9.586l-1.239-.756zM16 4.697v7.104l-5.803-3.558L16 4.697z"/></svg> Email: <a href="mailto:soilihi.abdoulhalim@outlook.fr" class="text-white hover-text-neon-green text-decoration-none">soilihi.abdoulhalim@outlook.fr</a></p>
        <p class="text-secondary text-lg"><svg class="me-2 text-neon-green" width="1em" height="1em" viewBox="0 0 16 16" fill="currentColor" xmlns="http://www.w3.org/2000/svg"><path d="M0 1.146C0 .513.526 0 1.176 0h13.648c.65 0 1.176.513 1.176 1.146v13.708c0 .633-.526 1.146-1.176 1.146H1.176C.526 16 0 15.487 0 14.854V1.146zm4.943 12.248V6.169h2.478v7.225H4.943zm-2.47-7.243c.827 0 1.488-.659 1.488-1.478 0-.818-.66-1.478-1.488-1.478-.827 0-1.488.659-1.488 1.478 0 .818.66 1.478 1.488 1.478zm6.504 3.097V14h2.478v-5.713c0-1.393.73-2.049 1.703-2.049 1.292 0 1.838.924 1.838 2.298V14H16V8.16c0-2.22-.958-3.535-2.928-3.535-1.434 0-2.043.793-2.394 1.246V4.31H9.006z"/></svg> LinkedIn: <a href="https://www.linkedin.com/in/abdoulhalim-soilihi-b749779b" target="_blank" class="text-white hover-text-neon-green text-decoration-none">abdoulhalim-soilihi-b749779b</a></p>
      </div>
    </div>
  `,
  // MODIFIÉ : On récupère 'emit' depuis le contexte du setup
  setup(props, { emit }) {
    const name = ref('');
    const email = ref('');
    const message = ref('');
    const status = ref('');

    const handleSubmit = (e) => {
      e.preventDefault();

      // MODIFIÉ : Créer l'objet du message
      const messageData = {
        name: name.value,
        email: email.value,
        message: message.value
      };

      // MODIFIÉ : Émettre l'événement avec les données du formulaire
      emit('message-submitted', messageData);

      // Le reste de la logique reste pour l'expérience utilisateur
      status.value = 'Message envoyé avec succès !';
      name.value = '';
      email.value = '';
      message.value = '';
      setTimeout(() => status.value = '', 5000);
    };

    return { name, email, message, status, handleSubmit };
  }
};
</script>

<style scoped>
/* Les styles restent inchangés */
:root {
  --neon-green: #39FF14;
  --dark-background: #000000;
  --dark-card: #1a1a1a;
}

.font-sans {
  font-family: 'Inter', sans-serif;
}

.bg-dark {
  background-color: var(--dark-background) !important;
}

.text-neon-green {
  color: var(--neon-green) !important;
}

.btn-neon-green {
  background-color: var(--neon-green) !important;
  border-color: var(--neon-green) !important;
}

.btn-neon-green:hover {
  background-color: rgba(57, 255, 20, 0.8) !important;
  border-color: rgba(57, 255, 20, 0.8) !important;
}

.bg-dark-card {
  background-color: var(--dark-card) !important;
}

.border-opacity-25 {
  border-color: rgba(255, 255, 255, 0.25) !important;
}

.h-300px {
  height: 300px;
}

@media (min-width: 576px) {
  .h-sm-400px {
    height: 400px;
  }
}

@media (min-width: 992px) {
  .w-lg-40 {
    width: 40% !important;
  }
}

@media (min-width: 992px) {
  .w-lg-400px {
    width: 400px !important;
  }
}

.developer-badge {
  font-size: 1.5rem;
  letter-spacing: 0.15em;
}

@media (min-width: 768px) {
  .developer-badge {
    font-size: 2.5rem;
  }
}

@media (min-width: 992px) {
  .developer-badge {
    font-size: 3.75rem;
  }
}

.hover-border-neon-green:hover {
  border-color: var(--neon-green) !important;
}

.group-hover-translate-x-2:hover {
  transform: translateX(0.5rem);
}

.transform-translate-y-50 {
  transform: translateY(-50%);
}

.translate-x-50 {
  transform: translateX(50%);
}

.rotate-90 {
  transform: rotate(90deg);
}

.origin-bottom-left {
  transform-origin: bottom left;
}

.text-nowrap {
  white-space: nowrap;
}

.letter-spacing-1 {
  letter-spacing: 0.1em;
}

.resize-vertical {
  resize: vertical;
}

.form-control.focus-border-neon-green:focus {
  border-color: var(--neon-green) !important;
  box-shadow: 0 0 0 0.25rem rgba(57, 255, 20, 0.25) !important;
}

@keyframes fade-in {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@keyframes fade-in-left {
  from {
    opacity: 0;
    transform: translateX(-50px);
  }

  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes fade-in-right {
  from {
    opacity: 0;
    transform: translateX(50px);
  }

  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes fade-in-up {
  from {
    opacity: 0;
    transform: translateY(20px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fade-in-down {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-in {
  animation: fade-in 0.8s ease-out forwards;
}

.animate-fade-in-left {
  animation: fade-in-left 0.8s ease-out forwards;
}

.animate-fade-in-right {
  animation: fade-in-right 0.8s ease-out forwards;
}

.animate-fade-in-up {
  animation: fade-in-up 0.8s ease-out forwards;
}

.animate-fade-in-down {
  animation: fade-in-down 0.8s ease-out forwards;
}

.animation-delay-200 {
  animation-delay: 0.2s;
}

.animation-delay-300 {
  animation-delay: 0.3s;
}

.animation-delay-400 {
  animation-delay: 0.4s;
}

.animation-delay-500 {
  animation-delay: 0.5s;
}

.animation-delay-600 {
  animation-delay: 0.6s;
}

.animation-delay-900 {
  animation-delay: 0.9s;
}

.nav-link {
  color: #ccc;
  font-size: 1.125rem;
  font-weight: 500;
  position: relative;
  transition: color 0.3s ease-in-out;
  padding: 0.5rem 1rem;
  text-decoration: none;
}

.nav-link:hover {
  color: var(--neon-green);
}

.nav-link.active {
  color: var(--neon-green);
}

.nav-link.active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: var(--neon-green);
}
.main-content-area {
  /* Espace pour les grands écrans (tablettes, ordinateurs)
     7rem est une valeur sûre (environ 112px). */
  padding-top: 7rem;
}

@media (max-width: 991.98px) {
  /* Pour les écrans plus petits où le menu peut prendre plus de place.
     140px est un bon point de départ. Vous pouvez augmenter ou
     diminuer cette valeur si nécessaire. */
  .main-content-area {
    padding-top: 140px;
  }
}
</style>
```