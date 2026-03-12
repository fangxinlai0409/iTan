<template>
  <section class="quick-cards">
    <RouterLink
      to="/protection"
      :class="['quick-card', { 'quick-card-home': variant === 'home' }]"
    >
      <div class="quick-card-body">
        <div class="card-main">
          <div class="card-top">
            <div class="icon-wrap">👕</div>
            <div>
              <h3>What to Wear</h3>
              <p class="subtitle">Based on today’s UV level</p>
            </div>
          </div>

          <div class="card-content">
            <p class="summary">
              Get practical protection advice before going outside.
            </p>

            <ul class="mini-list">
              <li>🧢 Hat recommendation</li>
              <li>🕶 Sunglasses guidance</li>
              <li>🧴 Sunscreen reminder</li>
              <li>👕 Clothing protection tips</li>
            </ul>
          </div>

          <div class="card-footer">
            <span class="link-text">View tips →</span>
          </div>
        </div>

        <div v-if="variant === 'home'" class="card-image">
          <img
            src="https://images.unsplash.com/photo-1515886657613-9f3515b0c78f?auto=format&fit=crop&w=800&q=80"
            alt="Sun-safe outdoor clothing"
          />
        </div>
      </div>
    </RouterLink>

    <RouterLink
      to="/news"
      :class="['quick-card', { 'quick-card-home': variant === 'home' }]"
    >
      <div class="quick-card-body">
        <div class="card-main">
          <div class="card-top">
            <div class="icon-wrap">📘</div>
            <div>
              <h3>Learn More</h3>
              <p class="subtitle">UV awareness and skin safety</p>
            </div>
          </div>

          <div class="card-content">
            <p class="summary">
              Explore useful information about UV risks and prevention.
            </p>

            <ul class="mini-list">
              <li>📊 Skin cancer statistics</li>
              <li>🌞 What UV index means</li>
              <li>🛡 Sun protection basics</li>
              <li>📚 Easy learning articles</li>
            </ul>
          </div>

          <div class="card-footer">
            <span class="link-text">Explore →</span>
          </div>
        </div>

        <div v-if="variant === 'home'" class="card-image">
          <img
            src="https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=800&q=80"
            alt="Young adults learning outdoors"
          />
        </div>
      </div>
    </RouterLink>
  </section>
</template>

<script setup>
import { RouterLink } from 'vue-router'

defineProps({
  variant: {
    type: String,
    default: 'default',
  },
})
</script>

<style scoped>
.quick-cards {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 1rem;
  height: 100%;
}

.quick-card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 240px;
  padding: 1.25rem;
  border-radius: 20px;
  border: 1.5px solid #facc15;
  background: linear-gradient(180deg, #fffef7 0%, #fffdfa 100%);
  box-shadow: 0 10px 24px rgba(15, 23, 42, 0.05);
  text-decoration: none;
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
}

.quick-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 16px 30px rgba(15, 23, 42, 0.08);
  border-color: #f59e0b;
}

.quick-card-body {
  display: block;
}

.quick-card-home .quick-card-body {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  align-items: center;
}

.card-main {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.card-top {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
}

.icon-wrap {
  width: 54px;
  height: 54px;
  border-radius: 999px;
  background: #fef08a;
  display: grid;
  place-items: center;
  font-size: 1.5rem;
  flex-shrink: 0;
}

h3 {
  margin: 0;
  font-size: 1.8rem;
  color: #1f2937;
  line-height: 1.1;
}

.subtitle {
  margin: 0.35rem 0 0;
  font-size: 1rem;
  color: #6b7280;
}

.card-content {
  margin-top: 1rem;
  flex: 1;
}

.summary {
  margin: 0 0 0.9rem;
  color: #475569;
  line-height: 1.5;
}

.mini-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
  color: #374151;
  font-size: 1rem;
}

.card-footer {
  margin-top: 1rem;
}

.link-text {
  font-weight: 700;
  color: #d97706;
  font-size: 1rem;
}

.card-image {
  height: 300px;
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 14px 28px rgba(15,23,42,0.12);
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

@media (max-width: 768px) {
  .quick-cards {
    grid-template-columns: 1fr;
  }

  .quick-card {
    min-height: 220px;
  }

  h3 {
    font-size: 1.5rem;
  }

  .quick-card-home .quick-card-body {
    grid-template-columns: 1fr;
  }

  .card-image {
    height: 180px;
  }
}
</style>