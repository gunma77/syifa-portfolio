<template>
  <section class="hero" id="hero">
    <div class="hero-inner">

      <!-- Kiri: teks -->
      <div class="hero-text">
        <p class="hero-eyebrow anim" data-delay="0">Creative & Communication</p>
        <h1 class="hero-name anim" data-delay="1">
          Syifa Adha<br>
          <em>Khoirunnisa</em>
        </h1>
        <p class="hero-desc anim" data-delay="2">
          Mahasiswa Ilmu Komunikasi dengan keahlian di bidang
          Desain Grafis, Fotografi, dan Videografi — lebih dari 3 tahun pengalaman.
        </p>
        <div class="hero-actions anim" data-delay="3">
          <a href="#projects" class="btn-primary">Lihat Karya</a>
          <a href="#contact" class="btn-secondary">Hubungi Saya</a>
        </div>

        <!-- Sosial -->
        <div class="hero-socials anim" data-delay="4">
          <a href="https://instagram.com/ss.adha" target="_blank" title="Instagram">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4.5"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>
          </a>
          <a href="https://youtube.com/@syifaadhaa" target="_blank" title="YouTube">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M22 8s-.3-2-1.2-2.8c-1.1-1.2-2.4-1.2-3-1.3C15.4 3.8 12 3.8 12 3.8s-3.4 0-5.8.2c-.6.1-1.9.1-3 1.3C2.3 6 2 8 2 8S1.8 10.3 1.8 12.5v2.1C1.8 16.8 2 19 2 19s.3 2 1.2 2.8c1.1 1.2 2.6 1.1 3.3 1.2C8.8 23.2 12 23.2 12 23.2s3.4 0 5.8-.3c.6-.1 1.9-.1 3-1.3.9-.8 1.2-2.8 1.2-2.8s.2-2.3.2-4.5v-2C22.2 10.3 22 8 22 8z"/><polygon points="10,8.5 16,12 10,15.5" fill="currentColor" stroke="none"/></svg>
          </a>
          <a href="https://www.tiktok.com/@beautifulblurrrrrr?_r=1&_t=ZS-97D4zksIS1N" target="_blank" title="TikTok">
            <svg viewBox="0 0 24 24" fill="currentColor"><path d="M19.6 3.3A4.9 4.9 0 0 1 14.8 0h-3.4v16.4a2.9 2.9 0 0 1-2.9 2.5 2.9 2.9 0 0 1-2.9-2.9 2.9 2.9 0 0 1 2.9-2.9c.3 0 .6 0 .8.1V9.6a6.3 6.3 0 0 0-.8-.1 6.3 6.3 0 0 0-6.3 6.3 6.3 6.3 0 0 0 6.3 6.3 6.3 6.3 0 0 0 6.3-6.3V8.2a8.2 8.2 0 0 0 4.9 1.6V6.5a4.9 4.9 0 0 1-3.1-3.2z"/></svg>
          </a>
          <a title="Email">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M2 7l10 7 10-7"/></svg>
          </a>
        </div>
      </div>

      <!-- Kanan: foto -->
      <div class="hero-photo-wrap anim anim-photo" data-delay="2">
        <div class="hero-photo-bg"></div>
        <div class="hero-photo">
          <img src="/images/ssadha.jpeg" alt="Syifa Adha Khoirunnisa" />
          <div class="photo-fallback">SAK</div>
        </div>
        <!-- Badge floating -->
        <div class="badge badge-top anim anim-badge" data-delay="4">✦ Desain Grafis</div>
        <div class="badge badge-bottom anim anim-badge" data-delay="5">✦ Content Creator</div>
      </div>

    </div>

    <!-- Scroll hint -->
    <div class="scroll-hint anim" data-delay="6">
      <span>scroll</span>
      <div class="scroll-line"></div>
    </div>
  </section>
</template>

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  // Respect prefers-reduced-motion
  const reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (reduced) {
    document.querySelectorAll('.anim').forEach(el => el.classList.add('visible'))
    return
  }

  const BASE_DELAY = 100  // ms per step
  const elements = document.querySelectorAll('#hero .anim')

  elements.forEach(el => {
    const step = parseInt(el.dataset.delay ?? 0)
    const delay = step * BASE_DELAY + 80 // offset kecil supaya tidak langsung
    setTimeout(() => {
      el.classList.add('visible')
    }, delay)
  })
})
</script>

<style scoped>
/* ─────────────────────────────────────
   ENTRANCE ANIMATION
───────────────────────────────────── */

/* State awal: tersembunyi */
.anim {
  opacity: 0;
  transform: translateY(22px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1);
}

/* Foto: muncul dari kanan + scale */
.anim-photo {
  transform: translateX(28px) scale(0.97);
  transition:
    opacity 0.75s cubic-bezier(0.22, 1, 0.36, 1),
    transform 0.75s cubic-bezier(0.22, 1, 0.36, 1);
}

/* Badge: muncul dengan scale kecil */
.anim-badge {
  transform: scale(0.88) translateY(6px);
  transition:
    opacity 0.5s cubic-bezier(0.34, 1.56, 0.64, 1),
    transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

/* State visible */
.anim.visible {
  opacity: 1;
  transform: none;
}

/* ─────────────────────────────────────
   LAYOUT
───────────────────────────────────── */
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-top: var(--nav-h);
  background: var(--bg);
  position: relative;
  overflow: hidden;
}

.hero::before {
  content: '';
  position: absolute;
  top: -100px;
  right: -100px;
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, #fce4ef 0%, transparent 70%);
  pointer-events: none;
}

.hero-inner {
  max-width: 1100px;
  margin: 0 auto;
  padding: 4rem 2rem;
  display: flex;
  align-items: center;
  gap: 5rem;
}

/* ── Teks kiri ── */
.hero-text {
  flex: 1;
}

.hero-eyebrow {
  font-size: 0.8rem;
  font-weight: 500;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--pink-dark);
  margin-bottom: 1rem;
}

.hero-name {
  font-family: var(--font-display);
  font-size: clamp(2.8rem, 5vw, 4.2rem);
  font-weight: 600;
  line-height: 1.1;
  color: var(--text);
  margin-bottom: 1.5rem;
}

.hero-name em {
  font-style: italic;
  color: var(--pink-dark);
}

.hero-desc {
  font-size: 1rem;
  color: var(--text-muted);
  max-width: 420px;
  line-height: 1.7;
  margin-bottom: 2rem;
}

.hero-actions {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  margin-bottom: 2.5rem;
}

.btn-primary {
  background: var(--pink);
  color: #fff;
  padding: 0.75rem 1.8rem;
  border-radius: 999px;
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 500;
  transition: background 0.2s, transform 0.15s;
}

.btn-primary:hover {
  background: var(--pink-dark);
  transform: translateY(-1px);
}

.btn-secondary {
  border: 1px solid var(--border);
  color: var(--text);
  padding: 0.75rem 1.8rem;
  border-radius: 999px;
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 400;
  transition: border-color 0.2s, transform 0.15s;
}

.btn-secondary:hover {
  border-color: var(--pink);
  color: var(--pink-dark);
  transform: translateY(-1px);
}

.hero-socials {
  display: flex;
  gap: 1rem;
}

.hero-socials a {
  width: 38px;
  height: 38px;
  border: 1px solid var(--border);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-muted);
  transition: border-color 0.2s, color 0.2s;
}

.hero-socials a:hover {
  border-color: var(--pink);
  color: var(--pink-dark);
}

.hero-socials svg {
  width: 16px;
  height: 16px;
}

/* ── Foto kanan ── */
.hero-photo-wrap {
  position: relative;
  flex-shrink: 0;
}

.hero-photo-bg {
  position: absolute;
  inset: 16px -16px -16px 16px;
  border-radius: 40% 60% 60% 40% / 50% 40% 60% 50%;
  background: var(--pink-light);
  z-index: 0;
}

.hero-photo {
  position: relative;
  z-index: 1;
  width: 340px;
  height: 400px;
  border-radius: 40% 60% 60% 40% / 50% 40% 60% 50%;
  overflow: hidden;
  background: var(--border);
}

.hero-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.photo-fallback {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-display);
  font-size: 3rem;
  font-weight: 600;
  color: var(--pink-dark);
  background: var(--pink-light);
}

.hero-photo img + .photo-fallback {
  display: none;
}

/* Badge floating */
.badge {
  position: absolute;
  background: #fff;
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 0.4rem 1rem;
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--pink-dark);
  white-space: nowrap;
  box-shadow: 0 2px 12px rgba(242, 167, 195, 0.15);
  z-index: 2;
}

.badge-top {
  top: 24px;
  right: -20px;
}

.badge-bottom {
  bottom: 40px;
  left: -30px;
}

/* Scroll hint */
.scroll-hint {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  color: var(--text-muted);
  font-size: 0.7rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

/* Override: scroll-hint visible state harus tetap di tengah */
.scroll-hint.visible {
  transform: translateX(-50%) !important;
}

.scroll-line {
  width: 1px;
  height: 40px;
  background: linear-gradient(to bottom, var(--pink), transparent);
  animation: scrollDown 1.5s ease-in-out infinite;
}

@keyframes scrollDown {
  0%   { transform: scaleY(0); transform-origin: top; }
  50%  { transform: scaleY(1); transform-origin: top; }
  51%  { transform: scaleY(1); transform-origin: bottom; }
  100% { transform: scaleY(0); transform-origin: bottom; }
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .hero-inner {
    flex-direction: column-reverse;
    gap: 3rem;
    padding: 3rem 1.5rem;
    text-align: center;
  }

  .hero-desc {
    max-width: 100%;
  }

  .hero-actions,
  .hero-socials {
    justify-content: center;
  }

  .hero-photo {
    width: 260px;
    height: 300px;
  }

  .badge-top  { right: -10px; }
  .badge-bottom { left: -10px; }
}

/* Reduce motion fallback */
@media (prefers-reduced-motion: reduce) {
  .anim, .anim-photo, .anim-badge {
    transition: none;
  }
}
</style>