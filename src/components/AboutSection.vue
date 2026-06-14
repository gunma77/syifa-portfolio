<template>
  <section class="about" id="about">
    <div class="about-inner">

      <!-- Kiri: foto polaroid -->
      <div class="about-visual">
        <div class="polaroid">
          <img src="/images/ssadha2.jpeg" alt="Syifa" />
          <div class="polaroid-fallback">✦</div>
          <p class="polaroid-caption">Syifa Adha Khoirunnisa</p>
        </div>
        <!-- Dekorasi -->
        <div class="deco-star deco-1">✦</div>
        <div class="deco-star deco-2">★</div>
        <div class="deco-dot"></div>
      </div>

      <!-- Kanan: konten -->
      <div class="about-content">
        <p class="section-eyebrow">A little about me</p>
        <h2 class="section-title">Halo, Saya <em>Syifa!</em></h2>

        <p class="about-bio">
          Mahasiswa Ilmu Komunikasi Semester 4 di Bhakti Kencana University.
          Saya memiliki ketertarikan dan keahlian di bidang desain grafis, fotografi,
          dan videografi dengan pengalaman lebih dari <strong>3 tahun</strong>.
        </p>
        <p class="about-bio">
          Saya mampu berkomunikasi secara efektif serta bekerja sama dalam tim
          untuk mencapai tujuan bersama. Setiap karya mencerminkan komitmen saya
          terhadap kualitas, inovasi, dan pendekatan visual yang kuat.
        </p>

        <!-- Info cards -->
        <div class="about-cards">
          <div class="info-card" v-for="item in info" :key="item.label">
            <!-- <span class="info-icon">{{ item.icon }}</span> -->
             <span class="info-icon" v-html="item.icon"></span>
            <div>
              <p class="info-label">{{ item.label }}</p>
              <p class="info-value">{{ item.value }}</p>
            </div>
          </div>
        </div>

        <!-- Social links -->
        <div class="about-socials">
          <a v-for="s in socials" :key="s.label" :href="s.href" target="_blank" class="social-pill">
            <span class="social-icon" v-html="s.icon"></span>
            {{ s.label }}
          </a>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup>
const info = [
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M22 10v6M2 10l10-5 10 5-10 5-10-5z"/><path d="M6 12v5c0 1.7 2.7 3 6 3s6-1.3 6-3v-5"/></svg>`,
    label: 'Universitas', value: 'Bhakti Kencana University'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>`,
    label: 'Program Studi', value: 'Ilmu Komunikasi'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>`,
    label: 'Pengalaman', value: '3+ Tahun'
  },
  {
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>`,
    label: 'Lokasi', value: 'Bandung, Indonesia'
  },
]

const socials = [
  {
    label: '@ss.adha',
    href: 'https://instagram.com/ss.adha',
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4.5"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>`
  },
  {
    label: '@syifaadhaa',
    href: 'https://youtube.com/@syifaadhaa',
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M22 8s-.3-2-1.2-2.8c-1.1-1.2-2.4-1.2-3-1.3C15.4 3.8 12 3.8 12 3.8s-3.4 0-5.8.2c-.6.1-1.9.1-3 1.3C2.3 6 2 8 2 8S1.8 10.3 1.8 12.5v2.1C1.8 16.8 2 19 2 19s.3 2 1.2 2.8c1.1 1.2 2.6 1.1 3.3 1.2C8.8 23.2 12 23.2 12 23.2s3.4 0 5.8-.3c.6-.1 1.9-.1 3-1.3.9-.8 1.2-2.8 1.2-2.8s.2-2.3.2-4.5v-2C22.2 10.3 22 8 22 8z"/><polygon points="10,8.5 16,12 10,15.5" fill="currentColor" stroke="none"/></svg>`
  },
  {
    label: '@beautifulblurrrrr',
    href: 'https://tiktok.com/@beautifulblurrrrr',
    icon: `<svg viewBox="0 0 24 24" fill="currentColor"><path d="M19.6 3.3A4.9 4.9 0 0 1 14.8 0h-3.4v16.4a2.9 2.9 0 0 1-2.9 2.5 2.9 2.9 0 0 1-2.9-2.9 2.9 2.9 0 0 1 2.9-2.9c.3 0 .6 0 .8.1V9.6a6.3 6.3 0 0 0-.8-.1 6.3 6.3 0 0 0-6.3 6.3 6.3 6.3 0 0 0 6.3 6.3 6.3 6.3 0 0 0 6.3-6.3V8.2a8.2 8.2 0 0 0 4.9 1.6V6.5a4.9 4.9 0 0 1-3.1-3.2z"/></svg>`
  },
  {
    label: 'adhasyifa53@gmail.com',
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M2 7l10 7 10-7"/></svg>`
  },
]
</script>

<style scoped>
.about {
  padding: 7rem 2rem;
  background: var(--surface);
}

.about-inner {
  max-width: 1100px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 6rem;
}

/* ── Visual kiri ── */
.about-visual {
  position: relative;
  flex-shrink: 0;
}

.polaroid {
  background: #fff;
  padding: 1rem 1rem 2.5rem;
  box-shadow: 0 8px 40px rgba(0,0,0,0.1);
  border-radius: 4px;
  width: 280px;
  transform: rotate(-2deg);
  position: relative;
  z-index: 1;
}

.polaroid img {
  width: 100%;
  height: 320px;
  object-fit: cover;
  display: block;
  background: var(--pink-light);
}

.polaroid-fallback {
  position: absolute;
  top: 1rem;
  left: 1rem;
  right: 1rem;
  height: 320px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
  color: var(--pink);
  background: var(--pink-light);
  pointer-events: none;
}

.polaroid img + .polaroid-fallback {
  display: none;
}

.polaroid-caption {
  text-align: center;
  font-family: var(--font-display);
  font-style: italic;
  font-size: 0.85rem;
  color: var(--text-muted);
  margin-top: 0.5rem;
}

.deco-star {
  position: absolute;
  color: var(--pink);
  font-size: 1.4rem;
  z-index: 0;
}
.deco-1 { top: -20px; right: -16px; font-size: 1.8rem; }
.deco-2 { bottom: 10px; left: -20px; font-size: 1.1rem; opacity: 0.5; }

.deco-dot {
  position: absolute;
  bottom: -16px;
  right: 30px;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: var(--pink-light);
  z-index: 0;
}

/* ── Konten kanan ── */
.about-content {
  flex: 1;
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
  margin-bottom: 1.5rem;
}

.section-title em {
  font-style: italic;
  color: var(--pink-dark);
}

.about-bio {
  font-size: 0.95rem;
  color: var(--text-muted);
  line-height: 1.8;
  margin-bottom: 1rem;
}

.about-bio strong {
  color: var(--text);
  font-weight: 500;
}

/* Info cards */
.about-cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
   text-align: left; 
  gap: 0.75rem;
  margin: 2rem 0;
}

.info-card {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 0.75rem 1rem;
}

.info-icon {
  font-size: 1.2rem;
}

.info-label {
  font-size: 0.7rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.info-value {
  font-size: 0.85rem;
  font-weight: 500;
  color: var(--text);
}

/* Social pills */
.about-socials {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
}

.social-pill {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 0.4rem 0.9rem;
  font-size: 0.8rem;
  color: var(--text-muted);
  text-decoration: none;
  transition: border-color 0.2s, color 0.2s;
}

.social-pill:hover {
  border-color: var(--pink);
  color: var(--pink-dark);
}

.social-icon {
  width: 14px;
  height: 14px;
  display: flex;
  align-items: center;
}

.social-icon :deep(svg) {
  width: 14px;
  height: 14px;
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .about {
    padding: 5rem 1.5rem;
  }

  .about-inner {
    flex-direction: column;
    gap: 3rem;
    text-align: center;
  }

  .about-cards {
    grid-template-columns: 1fr;
  }

  .about-socials {
    justify-content: center;
  }

  .polaroid {
    width: 240px;
  }

  .polaroid img {
    height: 260px;
  }

  .polaroid-fallback {
    height: 260px;
  }
}

.info-icon :deep(svg) {
  width: 1.2rem;
  height: 1.2rem;
  stroke: var(--pink-dark);
}
</style>