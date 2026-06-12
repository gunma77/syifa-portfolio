<template>
  <section class="creator" id="content-creator">
    <div class="creator-inner">

      <!-- Header -->
      <div class="section-header">
        <p class="section-eyebrow">Content Creator & Videographer</p>
        <h2 class="section-title">Behind the <em>Camera</em></h2>
        <p class="section-sub">
          Dari konten edukasi hingga dokumentasi acara — setiap frame punya cerita.
        </p>
      </div>

      <!-- Tabs: Content Creator / Videographer -->
      <div class="creator-tabs">
        <button
          v-for="tab in tabs"
          :key="tab.key"
          class="tab-btn"
          :class="{ active: activeTab === tab.key }"
          @click="activeTab = tab.key"
        >
          {{ tab.label }}
        </button>
      </div>

      <!-- Content Creator -->
      <div v-if="activeTab === 'creator'" class="phones-showcase">
        <div class="phone-wrap" v-for="(acc, i) in accounts" :key="i">
          <div class="phone-frame">
            <div class="phone-screen">
              <img :src="acc.img" :alt="acc.handle" />
              <div class="phone-fallback">
                <span>{{ acc.emoji }}</span>
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
        <div class="video-hero">
          <div class="video-thumb">

            <video autoplay muted loop playsinline width="100%">
                <source src="/images/tes.mp4" type="video/mp4" />
                    Browser kamu tidak mendukung video.
            </video>
            
            <div class="video-fallback">🎬</div>
            <div class="play-btn">▶</div>
          </div>
          <div class="video-info">
            <span class="card-tag">Videographer & Editor</span>
            <h3 class="video-title">Relawan Gesit Bandung</h3>
            <p class="video-org">Kitabisa × Salam Setara × Relawan Gesit</p>
            <p class="video-desc">
              Merekam dan mengedit konten video kegiatan sosial relawan di Bandung.
              Video dipublikasikan di TikTok <strong>@gesit_kotabandung</strong> dan
              mengabadikan momen inspiratif bersama anak-anak serta komunitas.
            </p>
            <div class="video-stats">
              <div class="stat" v-for="s in stats" :key="s.label">
                <p class="stat-num">{{ s.num }}</p>
                <p class="stat-label">{{ s.label }}</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Phone mockup tambahan -->
        <div class="video-phones">
          <div class="vphone" v-for="(v, i) in videoClips" :key="i">
            <div class="vphone-screen">
              <img :src="v.img" :alt="v.caption" />
              <div class="vphone-fallback">🎥</div>
              <div class="vphone-overlay">
                <p>{{ v.caption }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const tabs = [
  { key: 'creator', label: '📱 Content Creator' },
  { key: 'video',   label: '🎬 Videographer & Editor' },
]
const activeTab = ref('creator')

const accounts = [
  {
    emoji: '🎓',
    img: 'src/images/ilkomBKU.jpeg',
    handle: '@ilmukomunikasi.bku',
    platform: 'TikTok — Prodi Ilmu Komunikasi',
    desc: 'Konten edukasi dan dokumentasi kegiatan akademik program studi Ilmu Komunikasi BKU.',
  },
  {
    emoji: '✨',
    img: 'src/images/sympnoia.jpeg',
    handle: '@sympnoia',
    platform: 'TikTok IPA 5',
    desc: 'Konten kreatif dengan total views 277K+ di video terpopuler. Kolaborasi dan cerita kehidupan.',
  },
  {
    emoji: '🏫',
    img: 'src/images/fa1ilkom.jpeg',
    handle: '@asavatraa',
    platform: 'TikTok FA1 ILKOM',
    desc: 'Dokumentasi kegiatan dan konten komunitas mahasiswa Ilmu Komunikasi angkatan FA1.',
  },
]

const stats = [
  { num: '3+',   label: 'Video Diproduksi' },
  { num: '100+', label: 'Total Views' },
  { num: '2024', label: 'Tahun Aktif' },
]

const videoClips = [
//   { img: 'src/images/gesit1.jpeg', caption: '"Hidup selain ngajarin kita..."' },
  { img: 'src/images/gesit1.jpeg'},
  { img: 'src/images/gesit2.jpeg'},
  { img: 'src/images/gesit3.jpeg'},
]
</script>

<style scoped>
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

.phone-fallback span { font-size: 2.5rem; }
.phone-fallback p { font-size: 0.75rem; color: var(--pink-dark); text-align: center; padding: 0 0.5rem; }

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
  gap: 3rem;
}

.video-hero {
  display: flex;
  gap: 3rem;
  align-items: flex-start;
}

.video-thumb {
  flex-shrink: 0;
  width: 360px;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
  aspect-ratio: 16 / 9;
  background: var(--pink-light);
  cursor: pointer;
}

.video-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.3s;
}

.video-thumb:hover img {
  transform: scale(1.04);
}

.video-fallback {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
  background: var(--pink-light);
}

.video-thumb img + .video-fallback { display: none; }

.play-btn {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  color: #fff;
  background: rgba(0,0,0,0.2);
  opacity: 0;
  transition: opacity 0.2s;
}

.video-thumb:hover .play-btn {
  opacity: 1;
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

/* Video phones row */
.video-phones {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
}

.vphone {
  width: 160px;
  flex-shrink: 0;
}

.vphone-screen {
  border-radius: 16px;
  overflow: hidden;
  aspect-ratio: 9 / 16;
  background: var(--pink-light);
  position: relative;
}

.vphone-screen img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.vphone-fallback {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  background: var(--pink-light);
}

.vphone-screen img + .vphone-fallback { display: none; }

.vphone-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.6), transparent);
  padding: 1rem 0.75rem 0.6rem;
}

.vphone-overlay p {
  font-size: 0.68rem;
  color: #fff;
  line-height: 1.4;
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .creator {
    padding: 5rem 1.5rem;
  }

  .phones-showcase {
    gap: 2rem;
  }

  .phone-frame {
    width: 160px;
  }

  .video-hero {
    flex-direction: column;
    gap: 2rem;
  }

  .video-thumb {
    width: 100%;
  }

  .video-phones {
    gap: 1rem;
  }

  .vphone {
    width: 120px;
  }
}
</style>