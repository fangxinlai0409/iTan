<template>
  <section class="home-page">
    <section class="hero">
    <div class="hero-copy">
        <p class="hero-eyebrow">Sun safety companion</p>
        <h1>Smarter daily UV protection</h1>
        <p class="hero-lead">
        Real-time UV alerts, clothing advice, and simple sun safety guidance for young adults.
        </p>

        <div class="hero-actions">
        <RouterLink to="/uv" class="primary-btn">Check Current UV</RouterLink>
        <RouterLink to="/protection" class="secondary-btn">What to Wear</RouterLink>
        </div>
    </div>

    <div class="hero-visual">
        <img
            class="hero-image"
            src="https://images.unsplash.com/photo-1607962837359-5e7e89f86776?q=80&w=1740&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
            alt="People enjoying outdoor sun safely"
        />
    </div>
    </section>

    <section :class="['uv-summary-card', uvColorClass]">
    <div class="uv-left">
        <span class="uv-location">Today in {{ state.location.value }}</span>
        <h2 class="uv-value">UV {{ uvData.uv_index }}</h2>
        <p class="uv-risk">{{ uvData.risk_level }} risk</p>
    </div>

    <div class="uv-right">
        <p class="uv-advice">{{ uvData.message }}</p>
        <RouterLink to="/uv" class="uv-details">
        View full UV details →
        </RouterLink>
    </div>
    </section>

    <QuickAccessCards variant="home" />

    <AustraliaHeatMap
      :regions="state.uvRegions.value"
      :loading="state.loading.regions"
      @refresh="actions.loadRegions"
    />
  </section>
</template>

<script setup>
import { computed } from 'vue'
import { RouterLink } from 'vue-router'
import QuickAccessCards from '../components/QuickAccessCards.vue'
import AustraliaHeatMap from '../components/AustraliaHeatMap.vue'

const props = defineProps({
  state: {
    type: Object,
    required: true,
  },
  actions: {
    type: Object,
    required: true,
  },
  variant: {
    type: String,
    default: 'default',
  },
})

const uvData = computed(() => {
  const reading = props.state.uvPayload.value?.reading
  const uv = Number(reading?.uv_index) || 0
  const risk = reading?.risk_level || 'Unknown'

  let message = 'UV data is currently unavailable.'

  if (uv < 3) {
    message = 'Low UV today. Basic protection is enough for short outdoor periods.'
  } else if (uv < 6) {
    message = 'Moderate UV today. Sunscreen and sunglasses are recommended.'
  } else if (uv < 8) {
    message = 'High UV today. Cover up and reduce direct sun exposure.'
  } else if (uv < 11) {
    message = 'Very high UV today. Seek shade and use full sun protection.'
  } else {
    message = 'Extreme UV today. Avoid direct sun and protect your skin carefully.'
  }

  return {
    uv_index: reading?.uv_index ?? '-',
    risk_level: risk,
    message,
  }
})

const uvColorClass = computed(() => {
  const uv = Number(props.state.uvPayload.value?.reading?.uv_index) || 0

  if (uv < 3) return 'uv-low'
  if (uv < 6) return 'uv-moderate'
  if (uv < 8) return 'uv-high'
  if (uv < 11) return 'uv-very-high'
  return 'uv-extreme'
})
</script>

<style scoped>
.home-page {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

/* ------------------------------
   Hero / Landing Section
------------------------------ */
.hero {
  display: grid;
  grid-template-columns: 52% 48%;
  gap: 2rem;
  align-items: stretch;
  padding: 2.5rem;
  border-radius: 28px;
  background:
    radial-gradient(circle at top right, rgba(255, 214, 102, 0.35), transparent 28%),
    linear-gradient(135deg, #fffef7 0%, #fef3c7 100%);
  box-shadow: 0 20px 45px rgba(15, 23, 42, 0.06);
  overflow: hidden;
}

.hero-copy {
  width: 100%;
  max-width: none;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.hero h1 {
  margin: 0.7rem 0 1rem;
  font-size: 4rem;
  line-height: 1.02;
  color: #111827;
  font-weight: 800;
  max-width: 20ch;
}

.hero-lead {
  margin: 0;
  font-size: 1.15rem;
  line-height: 1.75;
  color: #475569;
  max-width: 30rem;
}

.hero-visual {
  width: 100%;
  min-width: 0;
  display: flex;
  align-items: stretch;
  justify-content: stretch;
}

.hero-image {
  display: block;
  width: 100%;
  height: 100%;
  min-height: 380px;
  object-fit: cover;
  object-position: center;
  border-radius: 24px;
  box-shadow: 0 20px 45px rgba(0, 0, 0, 0.15);
}

.hero-eyebrow {
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.82rem;
  color: #6b7280;
  font-weight: 700;
}


.hero-actions {
  display: flex;
  gap: 0.9rem;
  margin-top: 1.5rem;
  flex-wrap: wrap;
}



.primary-btn,
.secondary-btn,
.summary-link {
  text-decoration: none;
  font-weight: 700;
  transition: transform 0.18s ease, box-shadow 0.18s ease, background 0.18s ease;
}

.primary-btn {
  background: #111827;
  color: white;
  padding: 0.85rem 1.15rem;
  border-radius: 14px;
  box-shadow: 0 10px 24px rgba(17, 24, 39, 0.18);
}

.primary-btn:hover {
  transform: translateY(-2px);
}

.secondary-btn {
  background: white;
  color: #111827;
  padding: 0.85rem 1.15rem;
  border-radius: 14px;
  border: 1px solid #e5e7eb;
}

.secondary-btn:hover {
  transform: translateY(-2px);
  background: #fffdf4;
}

.hero-highlights {
  margin: 1.5rem 0 0;
  padding: 0;
  list-style: none;
  display: grid;
  gap: 0.65rem;
}

.hero-highlights li {
  position: relative;
  padding-left: 1.35rem;
  color: #334155;
  line-height: 1.5;
}

.hero-highlights li::before {
  content: '•';
  position: absolute;
  left: 0;
  top: -0.02rem;
  color: #f59e0b;
  font-weight: 800;
}



.hero-card-main h2 {
  margin: 0.45rem 0 0.2rem;
  font-size: 3rem;
  line-height: 1;
  color: white;
}


/* ------------------------------
   Shared / summary card
------------------------------ */
.eyebrow {
  margin: 0;
  text-transform: uppercase;
  font-size: 0.82rem;
  color: #6b7280;
  letter-spacing: 0.05em;
}

.uv-summary-card {
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: center;
  gap: 2rem;

  padding: 2rem;
  border-radius: 24px;

  color: white;

  box-shadow: 0 24px 50px rgba(30,58,138,0.25);
}

.uv-low {
  background: linear-gradient(135deg,#22c55e,#16a34a);
}

.uv-moderate {
  background: linear-gradient(135deg,#fde047,#facc15);
  color: #1f2937;
}

.uv-moderate .uv-details {
  color: #b45309;
}

.uv-high {
  background: linear-gradient(135deg,#f97316,#ea580c);
}

.uv-very-high {
  background: linear-gradient(135deg,#ef4444,#dc2626);
}

.uv-extreme {
  background: linear-gradient(135deg,#7c3aed,#6d28d9);
}

.uv-summary-card:hover {
  transform: translateY(-2px);
  transition: all 0.2s ease;
}

.uv-location {
  opacity: 0.8;
  font-size: 0.9rem;
}

.uv-value {
  margin: 0.3rem 0;
  font-size: 3rem;
  font-weight: 800;
}

.uv-risk {
  margin: 0;
  font-size: 1.1rem;
  opacity: 0.9;
}

.uv-advice {
  margin: 0;
  max-width: 360px;
}

.uv-details {
  color: #fde68a;
  font-weight: 600;
  text-decoration: none;
}

.uv-left {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.uv-right {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.8rem;
  max-width: 420px;
}

.uv-location {
  opacity: 0.8;
  font-size: 0.85rem;
  letter-spacing: 0.05em;
}

.uv-value {
  font-size: 3.2rem;
  font-weight: 800;
  margin: 0;
}

.uv-risk {
  font-size: 1.1rem;
  opacity: 0.9;
}

.uv-advice {
  text-align: right;
  line-height: 1.6;
}

.uv-details {
  color: #fde68a;
  font-weight: 700;
  text-decoration: none;
}
/* ------------------------------
   Responsive
------------------------------ */
@media (max-width: 980px) {
  .hero {
    grid-template-columns: 1fr;
    padding: 1.6rem;
  }

  .hero h1 {
    font-size: 2.3rem;
    max-width: none;
  }

  .hero-visual {
    min-height: 240px;
  }

  .hero-card-main {
    width: 100%;
  }

  .card-one,
  .card-two {
    position: static;
    margin-top: 0.8rem;
  }

  .summary-content {
    flex-direction: column;
    align-items: flex-start;
  }

    .uv-summary-card {
    flex-direction: column;
    align-items: flex-start;
    padding: 1.5rem;
  }

  .uv-right {
    align-items: flex-start;
    max-width: none;
  }

  .uv-advice {
    text-align: left;
  }
}
</style>