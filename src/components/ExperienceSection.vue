<template>
  <section class="experience" id="experience" ref="sectionRef">
    <div class="experience-inner">

      <!-- Header -->
      <div
        class="section-header anim anim-up"
        :class="{ visible: isVisible('header') }"
        data-key="header"
        style="--delay: 0s"
      >
        <p class="section-eyebrow">My Experience & Skills</p>
        <h2 class="section-title">Yang Sudah Saya <em>Jalani</em></h2>
      </div>

      <!-- Timeline -->
      <div class="timeline">
        <div
          class="timeline-item"
          v-for="(item, i) in experiences"
          :key="i"
          :class="{ right: i % 2 !== 0 }"
        >
          <!-- Dot -->
          <div
            class="timeline-dot anim anim-dot"
            :class="{ visible: isVisible('dot-' + i) }"
            :data-key="'dot-' + i"
            :style="`--delay: 0.1s`"
          ></div>

          <!-- Card: kiri slide dari kanan, kanan slide dari kiri -->
          <div
            class="timeline-card anim"
            :class="[i % 2 === 0 ? 'anim-left' : 'anim-right', { visible: isVisible('card-' + i) }]"
            :data-key="'card-' + i"
            :style="`--delay: 0.15s`"
          >
            <span class="card-tag">{{ item.tag }}</span>
            <h3 class="card-role">{{ item.role }}</h3>
            <p class="card-org">{{ item.org }}</p>
            <p class="card-period">{{ item.period }}</p>
            <p class="card-desc">{{ item.desc }}</p>
          </div>
        </div>

        <!-- Garis tengah — animasi grow dari atas ke bawah -->
        <div
          class="timeline-line anim anim-line"
          :class="{ visible: isVisible('line') }"
          data-key="line"
          style="--delay: 0.1s"
        ></div>
      </div>

    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const sectionRef = ref(null)
const visibleItems = ref(new Set())
let observer = null

const isVisible = (key) => visibleItems.value.has(String(key))

const experiences = [
  {
    tag: 'Internship',
    role: 'Staff Divisi Creative Production ',
    org: 'KOMINFO',
    period: '2026 – 2027',
    desc: 'Membuat visual feeds media sosial, merancang company profile, serta memproduksi aset kreatif visual untuk kebutuhan publikasi dan komunikasi digital.',
  },

  {
    tag: 'Internship',
    role: 'Social Media Intern',
    org: 'Communication Science Study Program — Bhakti Kencana University',
    period: '2025 – 2026',
    desc: 'Mengelola konten media sosial program studi, membuat desain visual, dan mendokumentasikan kegiatan akademik.',
  },
  {
    tag: 'Organisasi',
    role: 'Dept. Media — BEM FSOS',
    org: 'Badan Eksekutif Mahasiswa FSOS',
    period: '2024 – 2025',
    desc: 'Bertanggung jawab atas produksi konten dan pengelolaan media sosial BEM tingkat fakultas.',
  },
  {
    tag: 'Organisasi',
    role: 'Kominfo — HIMA ILKOM',
    org: 'Himpunan Mahasiswa Ilmu Komunikasi',
    period: '2024 – 2025',
    desc: 'Mengelola komunikasi dan informasi himpunan, termasuk desain konten dan publikasi digital.',
  },
  {
    tag: 'Volunteer',
    role: 'Team Media',
    org: 'Relawan Gesit Bandung',
    period: '2024',
    desc: 'Membuat dan mengedit konten video untuk kegiatan relawan sosial bersama Kitabisa & Salam Setara.',
  },
  {
    tag: 'Freelance',
    role: 'Marketing & Admin Sosmed',
    org: 'Tutur Kami',
    period: '2024',
    desc: 'Menjalankan strategi pemasaran digital dan pengelolaan akun media sosial brand.',
  },
  {
    tag: 'Freelance',
    role: 'Admin Sosial Media',
    org: 'Berbagai Klien',
    period: '2023 – sekarang',
    desc: 'Pembuatan konten kreatif, penjadwalan posting, dan analisis engagement untuk berbagai klien.',
  },

  {
    tag: 'Freelance',
    role: 'Creative Director',
    org: 'Serene Soul Event Organizer',
    period: '2026',
    desc: 'Merancang visi kreatif, mengembangkan ide-ide inovatif, serta memastikan seluruh elemen visual dan komunikasi dalam proyek, acara, atau kampanye berjalan secara konsisten.',
  },
]

onMounted(() => {
  const reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (reduced) {
    const keys = ['header', 'line', ...experiences.flatMap((_, i) => [`dot-${i}`, `card-${i}`])]
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
    { threshold: 0.15 }
  )

  // Observe header
  const header = sectionRef.value?.querySelector('.section-header')
  if (header) observer.observe(header)

  // Observe setiap card, dot, dan garis
  sectionRef.value
    ?.querySelectorAll('.timeline-card, .timeline-dot, .timeline-line')
    .forEach(el => observer.observe(el))
})

onUnmounted(() => {
  observer?.disconnect()
})
</script>

<style scoped>
/* ─────────────────────────────────────
   ENTRANCE ANIMATION
───────────────────────────────────── */

/* Header: fade + slide atas */
.anim-up {
  opacity: 0;
  transform: translateY(20px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

/* Card kiri (genap): slide dari kanan */
.anim-left {
  opacity: 0;
  transform: translateX(36px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

/* Card kanan (ganjil): slide dari kiri */
.anim-right {
  opacity: 0;
  transform: translateX(-36px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

/* Dot: scale dari nol dengan sedikit bounce */
.anim-dot {
  opacity: 0;
  transform: translate(-50%, -50%) scale(0);
  transition:
    opacity 0.4s cubic-bezier(0.34, 1.6, 0.64, 1) var(--delay, 0s),
    transform 0.4s cubic-bezier(0.34, 1.6, 0.64, 1) var(--delay, 0s);
}

/* Garis: grow dari atas ke bawah */
.anim-line {
  transform-origin: top center;
  opacity: 1;
}

/* State visible */
.anim.visible {
  opacity: 1;
  transform: none;
}

/* Dot visible — preserve translate */
.anim-dot.visible {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
}

/* ─────────────────────────────────────
   LAYOUT
───────────────────────────────────── */
.experience {
  padding: 7rem 2rem;
  background: var(--surface);
}

.experience-inner {
  max-width: 1100px;
  margin: 0 auto;
}

/* Header */
.section-header {
  text-align: center;
  margin-bottom: 4rem;
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

/* ── Timeline ── */
.timeline {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 2.5rem;
}

/* Garis vertikal tengah */
.timeline-line {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  width: 1px;
  background: var(--border);
  z-index: 0;
  transform: translateX(-50%) scaleY(0);
  transform-origin: top center;
  transition: transform 1.5s cubic-bezier(0.22, 1, 0.36, 1) 0.1s;
}

.timeline-line.visible {
  transform: translateX(-50%) scaleY(1);
}

.timeline-item {
  display: flex;
  justify-content: flex-end;
  padding-right: calc(50% + 2.5rem);
  position: relative;
  z-index: 1;
}

.timeline-item.right {
  justify-content: flex-start;
  padding-right: 0;
  padding-left: calc(50% + 2.5rem);
}

/* Dot di tengah */
.timeline-dot {
  position: absolute;
  top: 1.5rem;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--pink);
  border: 3px solid var(--surface);
  box-shadow: 0 0 0 1px var(--pink);
  z-index: 2;
}

/* Card */
.timeline-card {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 1.5rem;
  width: 100%;
  max-width: 420px;
}

.timeline-card.visible:hover {
  border-color: var(--pink);
  transform: translateY(-2px) !important;
  box-shadow: 0 8px 24px rgba(242, 167, 195, 0.12);
  transition:
    border-color 0.2s,
    transform 0.2s,
    box-shadow 0.2s;
}

.card-tag {
  display: inline-block;
  font-size: 0.68rem;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--pink-dark);
  background: var(--pink-light);
  padding: 0.2rem 0.65rem;
  border-radius: 999px;
  margin-bottom: 0.75rem;
}

.card-role {
  font-size: 1rem;
  font-weight: 600;
  color: var(--text);
  margin-bottom: 0.25rem;
}

.card-org {
  font-size: 0.82rem;
  color: var(--pink-dark);
  font-weight: 500;
  margin-bottom: 0.2rem;
}

.card-period {
  font-size: 0.75rem;
  color: var(--text-muted);
  margin-bottom: 0.75rem;
}

.card-desc {
  font-size: 0.85rem;
  color: var(--text-muted);
  line-height: 1.6;
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .experience {
    padding: 5rem 1.5rem;
  }

  .timeline-line {
    left: 16px;
  }

  .timeline-item,
  .timeline-item.right {
    justify-content: flex-start;
    padding-right: 0;
    padding-left: 3rem;
  }

  /* Di mobile semua card slide dari bawah saja */
  .anim-left,
  .anim-right {
    transform: translateY(20px);
  }

  .timeline-dot {
    left: 16px;
  }

  .timeline-card {
    max-width: 100%;
  }
}

/* Reduce motion fallback */
@media (prefers-reduced-motion: reduce) {
  .anim-up, .anim-left, .anim-right, .anim-dot {
    transition: none;
    opacity: 1;
    transform: none !important;
  }

  .timeline-dot {
    transform: translate(-50%, -50%) !important;
  }

  .timeline-line {
    transition: none;
    transform: translateX(-50%) scaleY(1) !important;
  }
}
</style>