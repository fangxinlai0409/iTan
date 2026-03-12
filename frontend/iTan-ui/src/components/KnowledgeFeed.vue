<template>
  <section class="panel knowledge-panel">
    <header class="feed-header">
      <div>
        <p class="eyebrow">Stay informed</p>
        <h2>Sun-aware stories</h2>
      </div>
      <div class="filters">
        <button
          v-for="tag in tags"
          :key="tag"
          :class="['filter-btn', { active: selectedTag === tag }]"
          @click="selectedTag = tag"
        >
          {{ tag }}
        </button>
      </div>
      <button type="button" class="ghost" @click="$emit('refresh')">Refresh</button>
    </header>

  <article class="featured" v-if="filteredItems.length">
    <div class="featured-content">
      <div class="featured-text">
        <p class="tag">{{ filteredItems[0].tag }}</p>
        <h3>{{ filteredItems[0].title }}</h3>
        <p class="featured-summary">{{ filteredItems[0].summary }}</p>
        <a :href="filteredItems[0].actionUrl" target="_blank" rel="noreferrer" class="link">
          Read more ↗
        </a>
      </div>

      <div class="featured-image">
        <img
          :src="filteredItems[0].imageUrl"
          :alt="filteredItems[0].title"
        />
      </div>
    </div>
  </article>

    <div class="story-grid">
      <article class="story-card" v-for="item in filteredItems.slice(1)" :key="item.id">
        <div class="story-card-content">
          <div class="story-text">
            <div class="story-meta">
              <p class="tag">{{ item.tag }}</p>
            </div>

            <h4>{{ item.title }}</h4>
            <p class="story-summary">{{ item.summary }}</p>

            <div class="story-footer">
              <span class="story-source">{{ item.source }}</span>
              <a :href="item.actionUrl" target="_blank" rel="noreferrer" class="story-link">
                Open ↗
              </a>
            </div>
          </div>

          <div class="story-image">
            <img
              :src="item.imageUrl"
              :alt="item.title"
            />
          </div>
        </div>
      </article>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  items: {
    type: Array,
    default: () => [],
  },
})

defineEmits(['refresh'])

const selectedTag = ref('All')

const tags = ['All', 'UV Alert', 'Tip', 'Lifestyle']

const filteredItems = computed(() => {
  if (selectedTag.value === 'All') return props.items
  return props.items.filter((i) => i.tag === selectedTag.value)
})
</script>

<style scoped>
.panel {
  background: white;
  border-radius: 20px;
  padding: 1.5rem;
  box-shadow: 0 20px 45px rgba(15, 23, 42, 0.08);
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.feed-header {
  display: grid;
  grid-template-columns: 1fr auto auto;
  gap: 1rem;
  align-items: center;
}

.eyebrow {
  margin: 0;
  text-transform: uppercase;
  font-size: 0.8rem;
  color: #6b7280;
  letter-spacing: 0.05em;
}

h2 {
  margin: 0.2rem 0 0;
}

.featured {
  background: linear-gradient(135deg, #eef2ff, #e0e7ff);
  border-radius: 20px;
  padding: 1.25rem;
  overflow: hidden;
}

.featured-content {
  display: grid;
  grid-template-columns: 1.15fr 0.85fr;
  gap: 1.2rem;
  align-items: center;
}

.featured-text {
  display: flex;
  flex-direction: column;
}

.featured-image {
  height: 220px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 12px 24px rgba(15, 23, 42, 0.08);
}

.featured-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.tag {
  font-size: 0.75rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #6366f1;
  margin: 0 0 0.3rem;
  font-weight: 700;
}

h3,
h4 {
  margin: 0.2rem 0 0.45rem;
  color: #111827;
}

.featured-summary {
  margin: 0 0 0.75rem;
  line-height: 1.6;
  color: #334155;
}

.link {
  font-weight: 700;
  color: #312e81;
  text-decoration: none;
}

.link:hover {
  text-decoration: underline;
}

.story-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

.story-card {
  border: 1px solid #e5e7eb;
  border-radius: 18px;
  padding: 1rem 1.05rem;
  background: linear-gradient(180deg, #ffffff 0%, #f8fafc 100%);
  transition: transform 0.18s ease, box-shadow 0.18s ease, border-color 0.18s ease;
}

.story-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 16px 28px rgba(15, 23, 42, 0.08);
  border-color: #cbd5e1;
}

.story-meta {
  margin-bottom: 0.1rem;
}

.story-summary {
  margin: 0;
  color: #475569;
  line-height: 1.6;
}

.story-footer {
  margin-top: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  flex-wrap: wrap;
}

.story-source {
  font-size: 0.95rem;
  color: #6b7280;
}

.story-link {
  font-weight: 700;
  color: #1d4ed8;
  text-decoration: none;
  padding: 0.3rem 0;
}

.story-link:hover {
  text-decoration: underline;
}

.story-card-content {
  display: grid;
  grid-template-columns: 1fr 220px;
  gap: 1.2rem;
  align-items: center;
}

.story-text {
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-width: 0;
}

.story-image {
  height: 140px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 20px rgba(15, 23, 42, 0.08);
}

.story-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.filters {
  display: flex;
  gap: 0.6rem;
  margin-bottom: 0.5rem;
}

.filter-btn {
  border: 1px solid #e5e7eb;
  background: white;
  padding: 0.35rem 0.8rem;
  border-radius: 999px;
  font-size: 0.85rem;
  cursor: pointer;
  color: #374151;
}

.filter-btn:hover {
  background: #f1f5f9;
}

.filter-btn.active {
  background: #1e3a8a;
  color: white;
  border-color: #1e3a8a;
}

.ghost {
  border: 1px solid #e5e7eb;
  background: transparent;
  border-radius: 10px;
  padding: 0.4rem 0.9rem;
  cursor: pointer;
  color: #1f2937;
  font-weight: 600;
}

@media (max-width: 768px) {
  .feed-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.8rem;
  }

  .filters {
    flex-wrap: wrap;
    margin-bottom: 0;
  }

  .featured-content {
    grid-template-columns: 1fr;
  }

  .featured-image {
    height: 180px;
  }

  .story-card-content {
    grid-template-columns: 1fr;
  }

  .story-image {
    height: 180px;
  }

  .story-footer {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>