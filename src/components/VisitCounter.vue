<template>
  <div class="visit-counter d-inline-flex align-items-center gap-1 small muted" :title="'Nombre total de visites'">
    <i class="bi bi-people-fill text-gradient"></i>
    <span v-if="loading" class="placeholder-glow"><span class="placeholder" style="width:40px;display:inline-block;height:0.8em;"></span></span>
    <span v-else>{{ count.toLocaleString('fr-FR') }} visites</span>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

/*
  Compteur de visites simple basé sur countapi.xyz (service public key-value).
  - Incrémente une seule fois par session (sessionStorage) pour éviter les multi increments sur SPA navigation.
  - Fallback: si l'incrément échoue, tente de récupérer la valeur actuelle.
  - Personnalisez namespace/key pour isoler vos stats.
*/

const loading = ref(true);
const count = ref(0);

const NAMESPACE = 'asoilihi-pro'; // à personnaliser si besoin
const KEY = 'site-visits';

async function fetchCount(increment = false) {
  const base = 'https://api.countapi.xyz';
  const endpoint = increment ? `hit/${NAMESPACE}/${KEY}` : `get/${NAMESPACE}/${KEY}`;
  try {
    const res = await fetch(`${base}/${endpoint}`);
    if(!res.ok) throw new Error('HTTP '+res.status);
    const data = await res.json();
    if(typeof data.value === 'number') count.value = data.value;
  } catch (e) {
    console.error('Erreur compteur visites:', e);
  } finally {
    loading.value = false;
  }
}

onMounted(async () => {
  const already = sessionStorage.getItem('visit-counted');
  await fetchCount(!already); // incrémente si pas encore compté cette session
  if(!already) sessionStorage.setItem('visit-counted','1');
});
</script>

<style scoped>
.visit-counter { font-variant-numeric: tabular-nums; opacity:.85; }
.visit-counter i { font-size: 1rem; }
.placeholder { background: rgba(255,255,255,0.15); border-radius:4px; }
/* Suppression de la règle de thème clair - force toujours le style sombre */
</style>
