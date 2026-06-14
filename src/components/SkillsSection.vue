<template>
  <section class="skills" id="skills" ref="sectionRef">
    <div class="skills-inner">

      <!-- Header -->
      <div class="section-header anim anim-up" style="--delay: 0s">
        <p class="section-eyebrow">What's in my tin case</p>
        <h2 class="section-title">Skills & <em>Tools</em></h2>
      </div>

      <!-- Soft skills -->
      <div class="skills-block">
        <h3 class="block-label anim anim-up" style="--delay: 0s">Soft Skills</h3>
        <div class="soft-skills">
          <div
            class="soft-card anim anim-up"
            v-for="(s, i) in softSkills"
            :key="s.label"
            style="--delay: 0s"
          >
            <span class="soft-icon" v-html="s.icon"></span>
            <p class="soft-label">{{ s.label }}</p>
            <p class="soft-desc">{{ s.desc }}</p>
          </div>
        </div>
      </div>

      <!-- Tools -->
      <div class="skills-block">
        <h3 class="block-label anim anim-up" style="--delay: 0s">Tools & Software</h3>
        <div class="tools-grid">
          <div
            class="tool-pill anim anim-pop"
            v-for="(t, i) in tools"
            :key="t.name"
            style="--delay: 0s"
          >
            <span class="tool-icon" v-html="t.icon"></span>
            <span class="tool-name">{{ t.name }}</span>
            <span class="tool-tag">{{ t.tag }}</span>
          </div>
        </div>
      </div>

      <!-- Keahlian utama -->
      <div class="skills-block">
        <h3 class="block-label anim anim-up" style="--delay: 0s">Keahlian Utama</h3>
        <div class="expertise-list">
          <div
            class="expertise-item anim anim-up"
            v-for="(e, i) in expertise"
            :key="e.label"
            style="--delay: 0s"
          >
            <span class="expertise-name">{{ e.label }}</span>
            <div class="dots">
              <span
                v-for="n in 5"
                :key="n"
                class="dot"
                :class="{ filled: n <= e.dots }"
              ></span>
            </div>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const sectionRef = ref(null)
let observer = null

onMounted(() => {
  const reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (reduced) {
    sectionRef.value?.querySelectorAll('.anim').forEach(el => el.classList.add('visible'))
    return
  }

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible')
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.15 }
  )

  sectionRef.value?.querySelectorAll('.anim').forEach(el => observer.observe(el))
})

onUnmounted(() => {
  observer?.disconnect()
})

const softSkills = [
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>`,
    label: 'Communication',
    desc: 'Komunikasi efektif secara verbal maupun visual'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>`,
    label: 'Team Work',
    desc: 'Kolaborasi dalam tim lintas divisi'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/></svg>`,
    label: 'Creativity',
    desc: 'Pendekatan kreatif dalam setiap karya'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>`,
    label: 'Time Management',
    desc: 'Mengelola deadline multi-proyek sekaligus'
  },
]

const tools = [
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M8 14s1.5 2 4 2 4-2 4-2"/><line x1="9" y1="9" x2="9.01" y2="9"/><line x1="15" y1="9" x2="15.01" y2="9"/></svg>`,
    name: 'Canva', tag: 'Design'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><polygon points="23 7 16 12 23 17 23 7"/><rect x="1" y="5" width="15" height="14" rx="2"/></svg>`,
    name: 'CapCut', tag: 'Video'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M12 19l7-7 3 3-7 7-3-3z"/><path d="M18 13l-1.5-7.5L2 2l3.5 14.5L13 18l5-5z"/><path d="M2 2l7.586 7.586"/><circle cx="11" cy="11" r="2"/></svg>`,
    name: 'ibisPaint', tag: 'Ilustrasi'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/><circle cx="12" cy="13" r="4"/></svg>`,
    name: 'Lightroom', tag: 'Foto'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><polyline points="4 7 4 4 20 4 20 7"/><line x1="9" y1="20" x2="15" y2="20"/><line x1="12" y1="4" x2="12" y2="20"/></svg>`,
    name: 'Pixellab', tag: 'Tipografi'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>`,
    name: 'Pinterest', tag: 'Referensi'
  },
]

const expertise = [
  { label: 'Desain Grafis',    dots: 5 },
  { label: 'Content Creation', dots: 5 },
  { label: 'Fotografi',        dots: 4 },
  { label: 'Videografi',       dots: 4 },
  { label: 'Social Media',     dots: 4 },
]
</script>

<style scoped>
/* ─────────────────────────────────────
   ENTRANCE ANIMATION
───────────────────────────────────── */

.anim-up {
  opacity: 0;
  transform: translateY(20px);
  transition:
    opacity 0.8s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.8s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.anim-pop {
  opacity: 0;
  transform: scale(0.85) translateY(8px);
  transition:
    opacity 0.65s cubic-bezier(0.34, 1.4, 0.64, 1) var(--delay, 0s),
    transform 0.65s cubic-bezier(0.34, 1.4, 0.64, 1) var(--delay, 0s);
}

.anim.visible {
  opacity: 1;
  transform: none;
}

/* ─────────────────────────────────────
   LAYOUT
───────────────────────────────────── */
.skills {
  padding: 7rem 2rem;
  background: var(--bg);
}

.skills-inner {
  max-width: 1100px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 4rem;
}

.section-header {
  text-align: center;
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

.block-label {
  font-size: 0.75rem;
  font-weight: 500;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-bottom: 1.25rem;
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.block-label::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--border);
}

/* ── Soft skills ── */
.soft-skills {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}

.soft-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 1.5rem 1.25rem;
  transition:
    border-color 0.2s,
    opacity 0.8s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.8s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.soft-card.visible:hover {
  border-color: var(--pink);
  transform: translateY(-3px) !important;
}

.soft-icon {
  display: flex;
  align-items: center;
  margin-bottom: 0.75rem;
}

.soft-icon :deep(svg) {
  width: 1.6rem;
  height: 1.6rem;
  stroke: var(--pink-dark);
}

.soft-label {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--text);
  margin-bottom: 0.35rem;
}

.soft-desc {
  font-size: 0.78rem;
  color: var(--text-muted);
  line-height: 1.5;
}

/* ── Tools ── */
.tools-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.tool-pill {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 0.55rem 1.1rem;
}

.tool-pill.visible:hover {
  border-color: var(--pink);
  background: var(--pink-light);
  transform: translateY(-2px) scale(1.03);
  transition:
    border-color 0.2s,
    background 0.2s,
    transform 0.2s;
}

.tool-icon {
  display: flex;
  align-items: center;
}

.tool-icon :deep(svg) {
  width: 1rem;
  height: 1rem;
  stroke: var(--pink-dark);
}

.tool-name {
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--text);
}

.tool-tag {
  font-size: 0.7rem;
  color: var(--pink-dark);
  background: var(--pink-light);
  padding: 0.1rem 0.5rem;
  border-radius: 999px;
}

/* ── Expertise dots ── */
.expertise-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 480px;
}

.expertise-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}

.expertise-name {
  font-size: 0.9rem;
  color: var(--text);
  min-width: 140px;
}

.dots {
  display: flex;
  gap: 6px;
}

.dot {
  width: 11px;
  height: 11px;
  border-radius: 50%;
  border: 1.5px solid var(--pink);
  background: transparent;
  transition:
    background 0.35s ease,
    border-color 0.35s ease;
}

.dot.filled {
  background: var(--pink);
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .skills {
    padding: 5rem 1.5rem;
  }

  .soft-skills {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 480px) {
  .soft-skills {
    grid-template-columns: 1fr;
  }
}

/* Reduce motion fallback */
@media (prefers-reduced-motion: reduce) {
  .anim-up, .anim-pop {
    transition: none;
    opacity: 1;
    transform: none;
  }
  .dot {
    transition: none;
  }
}
</style>