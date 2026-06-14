<template>
  <section class="projects" id="projects" ref="sectionRef">
    <div class="projects-inner">

      <!-- Header -->
      <div
        class="section-header anim anim-up"
        :class="{ visible: isVisible('header') }"
        data-key="header"
        style="--delay: 0s"
      >
        <p class="section-eyebrow">My Work</p>
        <h2 class="section-title">Karya <em>Terbaik</em></h2>
      </div>

      <!-- Filter tabs -->
      <div
        class="filter-tabs anim anim-up"
        :class="{ visible: isVisible('tabs') }"
        data-key="tabs"
        style="--delay: 0.1s"
      >
        <button
          v-for="tab in tabs"
          :key="tab"
          class="tab-btn"
          :class="{ active: activeTab === tab }"
          @click="activeTab = tab"
        >
          {{ tab }}
        </button>
      </div>

      <!-- Project grid -->
      <div class="projects-grid">
        <div
          class="project-card anim anim-up"
          :class="[{ featured: p.featured }, { visible: isVisible('card-' + p.title) }]"
          :data-key="'card-' + p.title"
          :style="`--delay: ${0.05 * i}s`"
          v-for="(p, i) in filtered"
          :key="p.title"
        >
          <!-- Gambar -->
          <div class="card-image">
            <img v-if="p.img" :src="p.img" :alt="p.title" />
            <div class="card-fallback">{{ p.emoji }}</div>
            <span class="card-category">{{ p.category }}</span>
          </div>

          <!-- Info -->
          <div class="card-body">
            <h3 class="card-title">{{ p.title }}</h3>
            <p class="card-desc">{{ p.desc }}</p>
            <div class="card-tags">
              <span v-for="tag in p.tags" :key="tag" class="tag">{{ tag }}</span>
            </div>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, watch } from 'vue'

const sectionRef = ref(null)
const visibleItems = ref(new Set())
let observer = null

const isVisible = (key) => visibleItems.value.has(String(key))

const tabs = ['Semua', 'Desain Grafis', 'Infografis', 'Book Cover']
const activeTab = ref('Semua')

const projects = [
  {
    emoji: '🎓',
    img: '/images/viskara.jpeg',
    title: 'VISKARA Graduation',
    category: 'Desain Grafis',
    desc: 'Desain identitas visual lengkap untuk acara wisuda VISKARA mulai dari logo, poster Coming Soon, hingga materi publikasi acara.',
    tags: ['Logo', 'Poster', 'Event'],
    featured: true,
  },
  {
    emoji: '📣',
    img: '/images/aspikom.jpeg',
    title: 'ASPIKOM — ISCF 2025',
    category: 'Desain Grafis',
    desc: 'Desain materi publikasi untuk International Student Communication Festival 2025, termasuk poster utama dan announcement juara.',
    tags: ['Poster', 'Kompetisi', 'Publikasi'],
    featured: true,
  },
  {
    emoji: '📊',
    img: '/images/infografisKoi.jpeg',
    title: 'Infografis Edukasi',
    category: 'Infografis',
    desc: 'Kumpulan infografis edukatif Feed the Fish, Pengaruh Media Sosial, Anti-Bullying, dan Desain Grafis Palestine.',
    tags: ['Infografis', 'Edukasi', 'Sosial'],
    featured: false,
  },
  {
    emoji: '📖',
    img: '/images/tujuhSemesta.jpeg',
    title: 'Toedjoeh Semesta',
    category: 'Book Cover',
    desc: 'Desain cover buku fiksi "Toedjoeh Semesta" yang diterbitkan oleh PT Arus Pedia Creative. Termasuk desain depan, belakang, dan layout.',
    tags: ['Book Cover', 'Layout', 'Penerbitan'],
    featured: true,
  },
  {
    emoji: '📱',
    img: '/images/fsos.jpeg',
    title: 'Design Feeds — BEM FSOS',
    category: 'Desain Grafis',
    desc: 'Desain feed Instagram untuk BEM FSOS mencakup konten hari besar nasional, Kartini Modern, Idul Fitri, dan kampanye sosial.',
    tags: ['Social Media', 'Feed Design', 'Branding'],
    featured: false,
  },
]

const filtered = computed(() =>
  activeTab.value === 'Semua'
    ? projects
    : projects.filter(p => p.category === activeTab.value)
)

// Saat tab berubah, observe ulang card-card yang baru muncul
watch(activeTab, () => {
  // Langsung visible karena sudah dalam viewport saat filter
  setTimeout(() => {
    sectionRef.value
      ?.querySelectorAll('.project-card')
      .forEach(el => {
        const key = el.dataset.key
        if (key) visibleItems.value = new Set([...visibleItems.value, key])
      })
  }, 50)
})

const observeCards = () => {
  sectionRef.value
    ?.querySelectorAll('.project-card')
    .forEach(el => {
      if (!el.dataset.key) return
      // Reset dulu biar animasi ulang jika diinginkan
      observer?.observe(el)
    })
}

onMounted(() => {
  const reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (reduced) {
    const keys = [
      'header', 'tabs',
      ...projects.map(p => `card-${p.title}`)
    ]
    visibleItems.value = new Set(keys)
    return
  }

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const key = entry.target.dataset.key
          visibleItems.value = new Set([...visibleItems.value, key])
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.12 }
  )

  // Observe header & tabs
  const header = sectionRef.value?.querySelector('.section-header')
  const tabs = sectionRef.value?.querySelector('.filter-tabs')
  if (header) observer.observe(header)
  if (tabs) observer.observe(tabs)

  // Observe cards
  observeCards()
})

onUnmounted(() => {
  observer?.disconnect()
})
</script>

<style scoped>
/* ─────────────────────────────────────
   ENTRANCE ANIMATION
───────────────────────────────────── */
.anim-up {
  opacity: 0;
  transform: translateY(24px);
  transition:
    opacity 0.55s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.55s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.anim.visible {
  opacity: 1;
  transform: none;
}

/* ─────────────────────────────────────
   LAYOUT
───────────────────────────────────── */
.projects {
  padding: 7rem 2rem;
  background: var(--bg);
}

.projects-inner {
  max-width: 1100px;
  margin: 0 auto;
}

/* Header */
.section-header {
  text-align: center;
  margin-bottom: 2.5rem;
}

.section-eyebrow {
  font-size: 0.78rem;
  font-weight: 500;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--pink-dark);
  margin-bottom: 0.75rem;
}

.section-title {
  font-family: var(--font-display);
  font-size: clamp(2rem, 3.5vw, 2.8rem);
  font-weight: 600;
  color: var(--text);
  line-height: 1.2;
}

.section-title em {
  font-style: italic;
  color: var(--pink-dark);
}

/* Filter tabs */
.filter-tabs {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 3rem;
}

.tab-btn {
  font-size: 0.8rem;
  font-weight: 400;
  padding: 0.45rem 1.1rem;
  border-radius: 999px;
  border: 1px solid var(--border);
  background: transparent;
  color: var(--text-muted);
  cursor: pointer;
  transition: all 0.2s;
  font-family: var(--font-body);
}

.tab-btn:hover {
  border-color: var(--pink);
  color: var(--pink-dark);
}

.tab-btn.active {
  background: var(--pink);
  border-color: var(--pink);
  color: #fff;
  font-weight: 500;
}

/* Grid */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.5rem;
}

.project-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 20px;
  overflow: hidden;
  transition:
    transform 1.25s, box-shadow 0.25s, border-color 0.25s,
    opacity 1.55s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
  display: flex;
  flex-direction: column;
}

.project-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 16px 40px rgba(242, 167, 195, 0.15);
  border-color: var(--pink);
}

/* Image area */
.card-image {
  position: relative;
  aspect-ratio: 3 / 4;
  background: var(--pink-light);
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.4s;
}

.project-card:hover .card-image img {
  transform: scale(1.04);
}

.card-fallback {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3.5rem;
  background: var(--pink-light);
}

.card-image img + .card-fallback {
  display: none;
}

.card-category {
  position: absolute;
  top: 0.75rem;
  left: 0.75rem;
  font-size: 0.68rem;
  font-weight: 500;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--pink-dark);
  background: rgba(255,255,255,0.9);
  padding: 0.25rem 0.65rem;
  border-radius: 999px;
  backdrop-filter: blur(4px);
}

/* Body */
.card-body {
  padding: 1.25rem 1.5rem 1.5rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.card-title {
  font-size: 1rem;
  font-weight: 600;
  color: var(--text);
}

.card-desc {
  font-size: 0.82rem;
  color: var(--text-muted);
  line-height: 1.6;
  flex: 1;
}

.card-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-top: 0.25rem;
}

.tag {
  font-size: 0.68rem;
  color: var(--pink-dark);
  background: var(--pink-light);
  padding: 0.2rem 0.55rem;
  border-radius: 999px;
}

/* ── Responsive ── */
@media (max-width: 900px) {
  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 600px) {
  .projects {
    padding: 5rem 1.5rem;
  }

  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 1rem;
  }
}

/* Reduce motion fallback */
@media (prefers-reduced-motion: reduce) {
  .anim-up {
    transition: none;
    opacity: 1;
    transform: none !important;
  }
}
</style>