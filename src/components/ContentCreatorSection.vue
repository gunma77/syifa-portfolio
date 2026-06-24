<template>
  <section class="creator" id="content-creator" ref="sectionRef">
    <div class="creator-inner">

      <!-- Header -->
      <div
        class="section-header anim anim-up"
        :class="{ visible: isVisible('header') }"
        data-key="header"
        style="--delay: 0s"
      >
        <p class="section-eyebrow">Content Creator & Videographer</p>
        <h2 class="section-title">Behind the <em>Camera</em></h2>
        <p class="section-sub">
          Dari konten edukasi hingga dokumentasi acara — setiap frame punya cerita.
        </p>
      </div>

      <!-- Tabs -->
      <div
        class="creator-tabs anim anim-up"
        :class="{ visible: isVisible('tabs') }"
        data-key="tabs"
        style="--delay: 0.1s"
      >
        <button
          v-for="tab in tabs"
          :key="tab.key"
          class="tab-btn"
          :class="{ active: activeTab === tab.key }"
          @click="activeTab = tab.key"
        >
          <span class="tab-icon" v-html="tab.icon"></span>
          {{ tab.label }}
        </button>
      </div>

      <!-- Content Creator -->
      <div v-if="activeTab === 'creator'" class="phones-showcase">
        <div
          class="phone-wrap anim anim-up"
          :class="{ visible: isVisible('phone-' + i) }"
          :data-key="'phone-' + i"
          :style="`--delay: ${0.08 * i}s`"
          v-for="(acc, i) in accounts"
          :key="i"
        >
          <div class="phone-frame">
            <div class="phone-screen">
              <img :src="acc.img" :alt="acc.handle" />
              <div class="phone-fallback">
                <span class="fallback-icon" v-html="acc.icon"></span>
                <p>{{ acc.handle }}</p>
              </div>
            </div>
          </div>
          <div class="phone-meta">
            <p class="acc-handle">{{ acc.handle }}</p>
            <p class="acc-platform">{{ acc.platform }}</p>
            <p class="acc-desc">{{ acc.desc }}</p>
          </div>
        </div>
      </div>

      <!-- Videographer -->
      <div v-if="activeTab === 'video'" class="video-showcase">

        <!-- Daftar video (hanya tampil sesuai visibleVideos) -->
        <div
          v-for="(video, i) in visibleVideos"
          :key="i"
          class="video-hero anim anim-up"
          :class="{ visible: isVisible('video-block-' + i) }"
          :data-key="'video-block-' + i"
          :style="`--delay: ${0.08 * i}s`"
        >
          <!-- YouTube: klik buka tab baru -->
          <a
            v-if="video.type === 'youtube'"
            class="video-thumb video-thumb--yt anim anim-scale"
            :class="{ visible: isVisible('video-thumb-' + i) }"
            :data-key="'video-thumb-' + i"
            :style="`--delay: ${0.05 * i}s`"
            :href="`https://www.youtube.com/watch?v=${video.youtubeId}`"
            target="_blank"
            rel="noopener noreferrer"
          >
            <img
              :src="`https://img.youtube.com/vi/${video.youtubeId}/hqdefault.jpg`"
              :alt="video.title"
            />
            <div class="play-btn">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="48" height="48">
                <path fill="#FF0000" d="M21.8 8s-.2-1.4-.8-2c-.8-.8-1.6-.8-2-.9C16.2 5 12 5 12 5s-4.2 0-7 .1c-.4.1-1.2.1-2 .9-.6.6-.8 2-.8 2S2 9.6 2 11.2v1.5c0 1.6.2 3.2.2 3.2s.2 1.4.8 2c.8.8 1.8.8 2.3.9C6.8 19 12 19 12 19s4.2 0 7-.2c.4-.1 1.2-.1 2-.9.6-.6.8-2 .8-2s.2-1.6.2-3.2v-1.5C22 9.6 21.8 8 21.8 8z"/>
                <path fill="#fff" d="M10 15l5.2-3L10 9v6z"/>
              </svg>
            </div>
            <span class="yt-badge">YouTube</span>
          </a>

          <!-- Cloudinary / file lokal: play di halaman -->
          <div
            v-else
            class="video-thumb anim anim-scale"
            :class="{ visible: isVisible('video-thumb-' + i) }"
            :data-key="'video-thumb-' + i"
            :style="`--delay: ${0.05 * i}s`"
            @click="togglePlayAt(i)"
          >
            <video
              :ref="el => { if (el) videoRefs[i] = el }"
              :src="video.src"
              loop
              playsinline
              preload="none"
            ></video>
            <div class="play-btn" :class="{ hidden: playingIndex === i }">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="40" height="40">
                <path d="M8 5v14l11-7L8 5z"/>
              </svg>
            </div>
          </div>

          <!-- Info -->
          <div
            class="video-info anim anim-left"
            :class="{ visible: isVisible('video-info-' + i) }"
            :data-key="'video-info-' + i"
            :style="`--delay: ${0.1 + 0.05 * i}s`"
          >
            <span class="card-tag">{{ video.tag }}</span>
            <h3 class="video-title">{{ video.title }}</h3>
            <p class="video-org">{{ video.org }}</p>
            <p class="video-desc" v-html="video.desc"></p>
            <div class="video-stats">
              <div
                class="stat anim anim-up"
                :class="{ visible: isVisible(`stat-${i}-${s.label}`) }"
                :data-key="`stat-${i}-${s.label}`"
                :style="`--delay: ${0.2 + 0.08 * si}s`"
                v-for="(s, si) in video.stats"
                :key="s.label"
              >
                <p class="stat-num">{{ s.num }}</p>
                <p class="stat-label">{{ s.label }}</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Tombol Tampilkan Lebih Banyak / Lebih Sedikit -->
        <div v-if="videos.length > INITIAL_SHOW" class="show-more-wrap">
          <!-- Gradient fade sebelum tombol (hanya saat collapsed) -->
          <div v-if="!showAll" class="show-more-fade"></div>
          <button class="show-more-btn" @click="toggleShowAll">
            <span v-if="!showAll">
              Tampilkan lebih banyak
              <span class="show-more-count">({{ videos.length - INITIAL_SHOW }} video lainnya)</span>
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="16" height="16">
                <path d="M7 10l5 5 5-5z"/>
              </svg>
            </span>
            <span v-else>
              Tampilkan lebih sedikit
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="16" height="16">
                <path d="M7 14l5-5 5 5z"/>
              </svg>
            </span>
          </button>
        </div>

      </div>

    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, watch, nextTick } from 'vue'

const sectionRef = ref(null)
const visibleItems = ref(new Set())
let observer = null

const isVisible = (key) => visibleItems.value.has(String(key))

const markVisible = (key) => {
  visibleItems.value = new Set([...visibleItems.value, key])
}

const observeAll = () => {
  if (!sectionRef.value || !observer) return
  sectionRef.value.querySelectorAll('[data-key]').forEach(el => {
    if (!visibleItems.value.has(el.dataset.key)) {
      observer.observe(el)
    }
  })
}

const tabs = [
  {
    key: 'creator',
    label: 'Content Creator',
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="16" height="16">
      <path d="M17 1H7C5.9 1 5 1.9 5 3v18c0 1.1.9 2 2 2h10c1.1 0 2-.9 2-2V3c0-1.1-.9-2-2-2zm-5 20c-.83 0-1.5-.67-1.5-1.5S11.17 18 12 18s1.5.67 1.5 1.5S12.83 21 12 21zm5-4H7V4h10v13z"/>
    </svg>`,
  },
  {
    key: 'video',
    label: 'Videographer & Editor',
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="16" height="16">
      <path d="M18 4l2 4h-3l-2-4h-2l2 4h-3l-2-4H8l2 4H7L5 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V4h-4z"/>
    </svg>`,
  },
]

const activeTab = ref('creator')

// ── Show more ────────────────────────────────────────────
const INITIAL_SHOW = 2
const showAll = ref(false)

const visibleVideos = computed(() =>
  showAll.value ? videos.value : videos.value.slice(0, INITIAL_SHOW)
)

async function toggleShowAll() {
  if (showAll.value) {
    // Collapse: scroll balik ke atas section video
    showAll.value = false
    await nextTick()
    const el = sectionRef.value?.querySelector('.video-showcase')
    if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' })
  } else {
    showAll.value = true
    await nextTick()
    observeAll()
  }
}

// ── Video state ──────────────────────────────────────────
const videoRefs = ref([])
const playingIndex = ref(null)

function togglePlayAt(i) {
  const el = videoRefs.value[i]
  if (!el) return
  if (playingIndex.value === i) {
    el.pause()
    playingIndex.value = null
  } else {
    videoRefs.value.forEach((v, idx) => { if (v && idx !== i) v.pause() })
    el.play()
    playingIndex.value = i
  }
}

// ── Data ─────────────────────────────────────────────────
const accounts = [
  {
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="40" height="40">
      <path d="M12 3L1 9l11 6 9-4.91V17h2V9L12 3zM5 13.18v4L12 21l7-3.82v-4L12 17l-7-3.82z"/>
    </svg>`,
    img: '/images/ilkomBKU.jpeg',
    handle: '@ilmukomunikasi.bku',
    platform: 'TikTok — Prodi Ilmu Komunikasi',
    desc: 'Konten edukasi dan dokumentasi kegiatan akademik program studi Ilmu Komunikasi BKU.',
  },
  {
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="40" height="40">
      <path d="M12 1l2.39 7.26H22l-6.19 4.5 2.35 7.24L12 15.9l-6.16 4.1 2.35-7.24L2 8.26h7.61L12 1z"/>
    </svg>`,
    img: '/images/sympnoia.jpeg',
    handle: '@sympnoia',
    platform: 'TikTok IPA 5',
    desc: 'Konten kreatif dengan total views 277K+ di video terpopuler. Kolaborasi dan cerita kehidupan.',
  },
  {
    icon: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="40" height="40">
      <path d="M5 13.18v4L12 21l7-3.82v-4L12 17l-7-3.82zM12 3L1 9l11 6 11-6-11-6z"/>
    </svg>`,
    img: '/images/fa1ilkom.jpeg',
    handle: '@asavatraa',
    platform: 'TikTok FA1 ILKOM',
    desc: 'Dokumentasi kegiatan dan konten komunitas mahasiswa Ilmu Komunikasi angkatan FA1.',
  },
]

// ── Tambah / edit video di sini ──────────────────────────
const videos = ref([
  {
    src: 'https://res.cloudinary.com/ds5nnop3u/video/upload/q_auto,f_auto/v1781837877/gesit_rkpfrn.mp4',
    title: 'Relawan Gesit Bandung',
    tag: 'Content Creator',
    org: 'GESIT Bandung',
    desc: 'Merancang konsep dan memproduksi konten dokumentasi kegiatan Dukungan Psikososial di Cisarua. Bertanggung jawab dalam pengambilan konten, penyusunan narasi visual, serta pembuatan materi publikasi yang mampu menggambarkan dampak dan tujuan kegiatan kepada masyarakat.',
    stats: [
      { num: '3+',   label: 'Video Diproduksi' },
      { num: '3000+', label: 'Total Views' },
      { num: '2026', label: 'Tahun Aktif' },
    ],
  },
  {
    src: 'https://res.cloudinary.com/ds5nnop3u/video/upload/q_auto,f_auto/v1781837877/cbsaui_e6h7q6.mp4',
    title: 'HIMA - CBSA UI',
    tag: 'Creative Video Editor',
    org: 'CBSA UI',
    desc: 'Meningkatkan kualitas konten melalui creative editing, termasuk penambahan sound effects, motion text, dan transisi yang mendukung storytelling. Mengubah materi dokumentasi menjadi video yang informatif, engaging, dan mudah dipahami oleh audiens. ',
    stats: [
      { num: '2',   label: 'Video Diproduksi' },
      { num: '6500+',  label: 'Total Views' },
      { num: '2025', label: 'Tahun Aktif' },
    ],
  },
  {
    src: 'https://res.cloudinary.com/ds5nnop3u/video/upload/q_auto,f_auto/v1781837931/ilkomBku_zdsvy4.mp4',
    title: 'Tim Kreatif Program Studi Ilmu Komunikasi Universitas Bhakti Kencana',
    tag: 'Content Creator Intern',
    org: 'Program Studi Ilmu Komunikasi Universitas Bhakti Kencana',
    desc: 'Melaksanakan kegiatan magang sebagai bagian dari tim kreatif program studi dengan tugas merancang ide konten, membuat materi publikasi digital, serta mendukung pengelolaan media sosial untuk meningkatkan engagement dan penyebaran informasi akademik.',
    stats: [
      { num: '1',   label: 'Video Diproduksi' },
      { num: '1000+',  label: 'Total Views' },
      { num: '2026', label: 'Tahun Aktif' },
    ],
  },
  {
    src: 'https://res.cloudinary.com/ds5nnop3u/video/upload/q_auto,f_auto/v1781837906/muftifauzi_vaz5tq.mp4',
    title: 'Asisten Dosen & Dokumentasi Kegiatan',
    tag: 'Teaching Assistant & Content Documentation',
    org: 'Universitas Bhakti Kencana',
    desc: 'Mendukung pelaksanaan kegiatan akademik sebagai asisten dosen sekaligus bertanggung jawab atas dokumentasi kegiatan. Mengelola proses pengambilan gambar, penyusunan konsep video, editing, hingga publikasi konten untuk kebutuhan dokumentasi dan promosi kegiatan akademik.',
    stats: [
      { num: '1',   label: 'Video Diproduksi' },
      { num: '850+',  label: 'Total Views' },
      { num: '2025', label: 'Tahun Aktif' },
    ],
  },
  {
    src: 'https://res.cloudinary.com/ds5nnop3u/video/upload/q_auto,f_auto/v1781837912/nunaeni_gba7wt.mp4',
    title: 'Video MKWU Agama – Universitas Bhakti Kencana',
    tag: 'Video Editor',
    org: 'Universitas Bhakti Kencana',
    desc: 'Mengedit video pembelajaran untuk Mata Kuliah Wajib Kurikulum Universitas (MKWU) Agama dengan fokus pada penyajian materi yang informatif, sistematis, dan menarik. Melakukan pengolahan visual, audio, serta elemen grafis untuk mendukung efektivitas penyampaian materi.',
    stats: [
      { num: '1',   label: 'Video Diproduksi' },
      { num: '1000+',  label: 'Total Views' },
      { num: '2025', label: 'Tahun Aktif' },
    ],
  },
  {
    src: 'https://res.cloudinary.com/ds5nnop3u/video/upload/q_auto,f_auto/v1781838018/akademikBku_agm5d7.mp4',
    title: 'PKM Internasional & Visit Campus Malaysia Universitas Bhakti Kencana',
    tag: 'Video Editor',
    org: 'PKM Internasional & Visit Campus Malaysia 2026',
    desc: 'Bertanggung jawab dalam proses editing video dokumentasi kegiatan Pengabdian Kepada Masyarakat (PKM) Internasional dan Visit Campus Malaysia. Mengemas dokumentasi kegiatan menjadi video yang profesional untuk kebutuhan publikasi institusi dan pelaporan program.',
    stats: [
      { num: '1',   label: 'Video Diproduksi' },
      { num: '3500+',  label: 'Total Views' },
      { num: '2026', label: 'Tahun Aktif' },
    ],
  },
  // ── YouTube videos ───────────────────────────────────────
  {
    type: 'youtube',
    youtubeId: 'dJrjbMA25KI',
    title: 'Video Project City Branding ',
    tag: 'Content Creator & Video Editor',
    org: 'City Branding Project'  ,
    desc: 'Merancang konsep konten serta melakukan proses editing video sebagai bagian dari proyek mata kuliah City Branding. Mengemas informasi mengenai identitas dan potensi suatu kota melalui pendekatan visual yang kreatif, komunikatif, dan sesuai dengan tujuan branding daerah.',
    stats: [
      { num: '1',   label: 'Video Diproduksi' },
      { num: '30+',  label: 'Total Views' },
      { num: '2026', label: 'Tahun Aktif' },
    ],
  },
  {
    type: 'youtube',
    youtubeId: 'FRapHwBRnFs',
    title: 'Video Recap Seminar Career Ready ',
    tag: 'Teaching Assistant & Media Documentation',
    org: 'Seminar Caree Ready',
    desc: 'Berperan sebagai asisten dosen sekaligus mengelola dokumentasi visual kegiatan akademik. Menghasilkan konten video yang informatif melalui proses produksi, editing, dan penyajian visual yang menarik.',
    stats: [
      { num: '1',   label: 'Video Diproduksi' },
      { num: '5000+',  label: 'Total Views' },
      { num: '2025', label: 'Tahun Aktif' },
    ],
  },
])

// ── Intersection Observer ────────────────────────────────
watch(activeTab, () => {
  showAll.value = false
  setTimeout(observeAll, 50)
})

onMounted(() => {
  const reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (reduced) {
    const allKeys = [
      'header', 'tabs',
      ...accounts.map((_, i) => `phone-${i}`),
      ...videos.value.flatMap((v, i) => [
        `video-block-${i}`,
        `video-thumb-${i}`,
        `video-info-${i}`,
        ...v.stats.map(s => `stat-${i}-${s.label}`),
      ]),
    ]
    visibleItems.value = new Set(allKeys)
    return
  }

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          markVisible(entry.target.dataset.key)
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.12 }
  )

  observeAll()
})

onUnmounted(() => {
  observer?.disconnect()
})
</script>

<style scoped>
/* ─────────────────────────────────────
   ENTRANCE ANIMATIONS
───────────────────────────────────── */
.anim-up {
  opacity: 0;
  transform: translateY(24px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.anim-left {
  opacity: 0;
  transform: translateX(-28px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.anim-scale {
  opacity: 0;
  transform: scale(0.92);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.anim.visible {
  opacity: 1;
  transform: none;
}

/* ─────────────────────────────────────
   LAYOUT
───────────────────────────────────── */
.creator {
  padding: 7rem 2rem;
  background: var(--surface);
}

.creator-inner {
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
  margin-bottom: 0.75rem;
}

.section-title em {
  font-style: italic;
  color: var(--pink-dark);
}

.section-sub {
  font-size: 0.9rem;
  color: var(--text-muted);
  max-width: 440px;
  margin: 0 auto;
  line-height: 1.6;
}

/* Tabs */
.creator-tabs {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 3.5rem;
}

.tab-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  font-size: 0.85rem;
  font-weight: 400;
  padding: 0.55rem 1.4rem;
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

.tab-icon {
  display: inline-flex;
  align-items: center;
  line-height: 1;
}

.tab-btn.active .tab-icon svg { fill: #fff; }
.tab-btn:not(.active) .tab-icon svg { fill: currentColor; }

/* ── Phone showcase (Content Creator) ── */
.phones-showcase {
  display: flex;
  justify-content: center;
  gap: 3rem;
  flex-wrap: wrap;
}

.phone-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
}

.phone-frame {
  width: 200px;
  border-radius: 36px;
  background: #1a1a1a;
  padding: 10px;
  box-shadow: 0 20px 60px rgba(0,0,0,0.15);
}

.phone-screen {
  border-radius: 28px;
  overflow: hidden;
  aspect-ratio: 9 / 16;
  background: var(--pink-light);
  position: relative;
}

.phone-screen img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.phone-fallback {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: var(--pink-light);
}

.fallback-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: var(--pink-dark);
}

.fallback-icon svg { fill: var(--pink-dark); }
.phone-fallback p {
  font-size: 0.75rem;
  color: var(--pink-dark);
  text-align: center;
  padding: 0 0.5rem;
}

.phone-screen img + .phone-fallback { display: none; }

.phone-meta {
  text-align: center;
  max-width: 200px;
}

.acc-handle {
  font-weight: 600;
  font-size: 0.9rem;
  color: var(--text);
}

.acc-platform {
  font-size: 0.75rem;
  color: var(--pink-dark);
  margin-bottom: 0.35rem;
}

.acc-desc {
  font-size: 0.78rem;
  color: var(--text-muted);
  line-height: 1.5;
}

/* ── Video showcase (Videographer) ── */
.video-showcase {
  display: flex;
  flex-direction: column;
  gap: 0;
  position: relative;
}

/* Setiap blok video */
.video-hero {
  display: flex;
  gap: 3rem;
  align-items: flex-start;
  padding: 3rem 0;
}

/* Separator antar video */
.video-hero + .video-hero {
  border-top: 1px solid var(--border);
}

.video-thumb {
  flex-shrink: 0;
  width: 220px;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
  aspect-ratio: 9 / 16;
  background: var(--pink-light);
  cursor: pointer;
}

.video-thumb video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.play-btn {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  background: rgba(0,0,0,0.25);
  transition: opacity 0.3s;
  pointer-events: none;
}

.play-btn.hidden { opacity: 0; }

@media (hover: hover) {
  .video-thumb:hover .play-btn.hidden { opacity: 0.6; }
}

.video-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
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
  width: fit-content;
}

.video-title {
  font-family: var(--font-display);
  font-size: 1.6rem;
  font-weight: 600;
  color: var(--text);
  line-height: 1.2;
}

.video-org {
  font-size: 0.82rem;
  color: var(--pink-dark);
  font-weight: 500;
}

.video-desc {
  font-size: 0.88rem;
  color: var(--text-muted);
  line-height: 1.7;
}

.video-stats {
  display: flex;
  gap: 2rem;
  margin-top: 0.5rem;
}

.stat-num {
  font-family: var(--font-display);
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--pink-dark);
}

.stat-label {
  font-size: 0.75rem;
  color: var(--text-muted);
}

/* ── YouTube thumbnail ── */
.video-thumb--yt {
  display: block;
  text-decoration: none;
}

.video-thumb--yt img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.yt-badge {
  position: absolute;
  top: 0.6rem;
  left: 0.6rem;
  background: #FF0000;
  color: #fff;
  font-size: 0.6rem;
  font-weight: 700;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  padding: 0.2rem 0.5rem;
  border-radius: 4px;
}

.video-thumb--yt:hover .play-btn { opacity: 0.85; }

/* ── Show More ── */
.show-more-wrap {
  position: relative;
  text-align: center;
  padding-top: 1rem;
}

/* Gradient fade di atas tombol saat collapsed */
.show-more-fade {
  position: absolute;
  top: -80px;
  left: 0;
  right: 0;
  height: 80px;
  background: linear-gradient(to bottom, transparent, var(--surface));
  pointer-events: none;
}

.show-more-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.85rem;
  font-weight: 500;
  padding: 0.65rem 1.6rem;
  border-radius: 999px;
  border: 1px solid var(--border);
  background: transparent;
  color: var(--text-muted);
  cursor: pointer;
  transition: all 0.2s;
  font-family: var(--font-body);
  margin-top: 1rem;
}

.show-more-btn:hover {
  border-color: var(--pink);
  color: var(--pink-dark);
  background: var(--pink-light);
}

.show-more-btn svg {
  fill: currentColor;
  flex-shrink: 0;
}

.show-more-count {
  color: var(--pink-dark);
  font-weight: 600;
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .creator { padding: 5rem 1.5rem; }
  .phones-showcase { gap: 2rem; }
  .phone-frame { width: 160px; }

  .video-hero {
    flex-direction: column;
    gap: 2rem;
    padding: 2.5rem 0;
  }

  .video-thumb { width: 100%; }
}

/* Reduce motion fallback */
@media (prefers-reduced-motion: reduce) {
  .anim-up, .anim-left, .anim-scale {
    transition: none;
    opacity: 1;
    transform: none !important;
  }
}
</style>