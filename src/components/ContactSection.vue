<template>
  <section class="contact" id="contact" ref="sectionRef">
    <div class="contact-inner">

      <!-- Kiri: teks & info -->
      <div class="contact-left">
        <p
          class="section-eyebrow anim anim-up"
          :class="{ visible: isVisible('eyebrow') }"
          data-key="eyebrow"
          style="--delay: 0s"
        >Get in Touch</p>

        <h2
          class="section-title anim anim-up"
          :class="{ visible: isVisible('title') }"
          data-key="title"
          style="--delay: 0.08s"
        >Let's Work <em>Together!</em></h2>

        <p
          class="contact-sub anim anim-up"
          :class="{ visible: isVisible('sub') }"
          data-key="sub"
          style="--delay: 0.14s"
        >
          Punya proyek desain, konten, atau kolaborasi yang menarik?
          Jangan ragu untuk menghubungi saya — mari kita wujudkan ide bersama.
        </p>

        <div class="contact-links">
          <a
            v-for="(c, i) in contacts"
            :key="c.label"
            :href="c.href"
            target="_blank"
            class="contact-item anim anim-left"
            :class="{ visible: isVisible('contact-' + i) }"
            :data-key="'contact-' + i"
            :style="`--delay: ${0.18 + i * 0.07}s`"
          >
            <div class="contact-icon" v-html="c.icon"></div>
            <div class="contact-meta">
              <p class="contact-platform">{{ c.platform }}</p>
              <p class="contact-label">{{ c.label }}</p>
            </div>
          </a>
        </div>
      </div>

      <!-- Kanan: form -->
      <div
        class="contact-right anim anim-right"
        :class="{ visible: isVisible('form') }"
        data-key="form"
        style="--delay: 0.1s"
      >
        <div class="form-card">
          <h3 class="form-title">
            Kirim Pesan
            <svg class="title-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" xmlns="http://www.w3.org/2000/svg">
              <rect x="2" y="4" width="20" height="16" rx="2"/>
              <path d="M2 7l10 7 10-7"/>
            </svg>
          </h3>

          <div class="form-group">
            <label>Nama</label>
            <input v-model="form.name" type="text" placeholder="Nama kamu" />
          </div>
          <div class="form-group">
            <label>Email</label>
            <input v-model="form.email" type="email" placeholder="email@kamu.com" />
          </div>
          <div class="form-group">
            <label>Subjek</label>
            <input v-model="form.subject" type="text" placeholder="Kolaborasi / Proyek / Lainnya" />
          </div>
          <div class="form-group">
            <label>Pesan</label>
            <textarea v-model="form.message" rows="4" placeholder="Tulis pesanmu..."></textarea>
          </div>

          <button class="btn-send" @click="sendEmail" :disabled="!isValid || sending">
            <span v-if="sending">Mengirim...</span>
            <span v-else-if="sent" class="sent-state">Terkirim!</span>
            <span v-else>Kirim Pesan</span>
          </button>
          <p v-if="error" class="error-msg">Gagal mengirim, coba lagi ya.</p>
        </div>
      </div>

    </div>

    <!-- Footer -->
    <div
      class="footer anim anim-up"
      :class="{ visible: isVisible('footer') }"
      data-key="footer"
      style="--delay: 0s"
    >
      <p class="footer-name">Syifa Adha Khoirunnisa</p>
      <p class="footer-copy">© {{ new Date().getFullYear() }} — All Rights Reserved</p>
    </div>

  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const sectionRef = ref(null)
const visibleItems = ref(new Set())
let observer = null

const isVisible = (key) => visibleItems.value.has(String(key))

const form = ref({ name: '', email: '', subject: '', message: '' })
const sent = ref(false)
const sending = ref(false)
const error = ref(false)

const isValid = computed(() =>
  form.value.name && form.value.email && form.value.message
)

async function sendEmail() {
  if (!isValid.value) return
  sending.value = true
  error.value = false

  try {
    const res = await fetch('https://formspree.io/f/mqeogzkj', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        name: form.value.name,
        email: form.value.email,
        subject: form.value.subject,
        message: form.value.message,
      })
    })

    if (res.ok) {
      sent.value = true
      form.value = { name: '', email: '', subject: '', message: '' }
      setTimeout(() => sent.value = false, 3000)
    } else {
      error.value = true
    }
  } catch (e) {
    error.value = true
  } finally {
    sending.value = false
  }
}

const contacts = [
  {
    platform: 'Email',
    label: 'adhasyifa53@gmail.com',
    href: 'mailto:adhasyifa53@gmail.com',
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M2 7l10 7 10-7"/></svg>`,
  },
  {
    platform: 'Instagram',
    label: '@ss.adha',
    href: 'https://instagram.com/ss.adha',
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4.5"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>`,
  },
  {
    platform: 'TikTok',
    label: '@beautifulblurrrrr',
    href: 'https://tiktok.com/@beautifulburrrr',
    icon: `<svg viewBox="0 0 24 24" fill="currentColor"><path d="M19.6 3.3A4.9 4.9 0 0 1 14.8 0h-3.4v16.4a2.9 2.9 0 0 1-2.9 2.5 2.9 2.9 0 0 1-2.9-2.9 2.9 2.9 0 0 1 2.9-2.9c.3 0 .6 0 .8.1V9.6a6.3 6.3 0 0 0-.8-.1 6.3 6.3 0 0 0-6.3 6.3 6.3 6.3 0 0 0 6.3 6.3 6.3 6.3 0 0 0 6.3-6.3V8.2a8.2 8.2 0 0 0 4.9 1.6V6.5a4.9 4.9 0 0 1-3.1-3.2z"/></svg>`,
  },
  {
    platform: 'YouTube',
    label: '@syifaadhaa',
    href: 'https://youtube.com/@syifaadhaa',
    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M22 8s-.3-2-1.2-2.8c-1.1-1.2-2.4-1.2-3-1.3C15.4 3.8 12 3.8 12 3.8s-3.4 0-5.8.2c-.6.1-1.9.1-3 1.3C2.3 6 2 8 2 8S1.8 10.3 1.8 12.5v2.1C1.8 16.8 2 19 2 19s.3 2 1.2 2.8c1.1 1.2 2.6 1.1 3.3 1.2C8.8 23.2 12 23.2 12 23.2s3.4 0 5.8-.3c.6-.1 1.9-.1 3-1.3.9-.8 1.2-2.8 1.2-2.8s.2-2.3.2-4.5v-2C22.2 10.3 22 8 22 8z"/><polygon points="10,8.5 16,12 10,15.5" fill="currentColor" stroke="none"/></svg>`,
  },
]

onMounted(() => {
  const reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (reduced) {
    const keys = [
      'eyebrow', 'title', 'sub', 'form', 'footer',
      ...contacts.map((_, i) => `contact-${i}`),
    ]
    visibleItems.value = new Set(keys)
    return
  }

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          visibleItems.value = new Set([...visibleItems.value, entry.target.dataset.key])
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.12 }
  )

  sectionRef.value?.querySelectorAll('[data-key]').forEach(el => observer.observe(el))
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
  transform: translateY(22px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

/* Contact links: slide dari kiri */
.anim-left {
  opacity: 0;
  transform: translateX(-28px);
  transition:
    opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

/* Form card: slide dari kanan */
.anim-right {
  opacity: 0;
  transform: translateX(32px);
  transition:
    opacity 0.65s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s),
    transform 0.65s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.anim.visible {
  opacity: 1;
  transform: none;
}

/* ─────────────────────────────────────
   LAYOUT
───────────────────────────────────── */
.contact {
  padding: 7rem 2rem 0;
  background: var(--bg);
}

.contact-inner {
  max-width: 1100px;
  margin: 0 auto;
  display: flex;
  gap: 5rem;
  align-items: flex-start;
  padding-bottom: 7rem;
}

/* ── Kiri ── */
.contact-left {
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
  margin-bottom: 1rem;
}

.section-title em {
  font-style: italic;
  color: var(--pink-dark);
}

.contact-sub {
  font-size: 0.9rem;
  color: var(--text-muted);
  line-height: 1.7;
  margin-bottom: 2.5rem;
  max-width: 380px;
}

.contact-links {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem 1.25rem;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 14px;
  text-decoration: none;
  transition: border-color 0.2s, transform 0.2s, box-shadow 0.2s,
              opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1) var(--delay, 0s);
}

.contact-item.visible:hover {
  border-color: var(--pink);
  transform: translateX(4px);
  box-shadow: 0 4px 16px rgba(242, 167, 195, 0.12);
}

.contact-icon {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: var(--pink-light);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--pink-dark);
}

.contact-icon :deep(svg) {
  width: 18px;
  height: 18px;
}

.contact-meta { flex: 1; }

.contact-platform {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--text-muted);
}

.contact-label {
  font-size: 0.88rem;
  font-weight: 500;
  color: var(--text);
}

/* ── Kanan: form ── */
.contact-right {
  flex: 1;
  width: 100%; /* ← tambah ini juga biar form full width */
}

.form-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 24px;
  padding: 2rem 2rem 2.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-title {
  font-family: var(--font-display);
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--text);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.title-icon {
  width: 20px;
  height: 20px;
  color: var(--pink-dark);
  flex-shrink: 0;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.form-group label {
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.form-group input,
.form-group textarea {
  font-family: var(--font-body);
  font-size: 0.9rem;
  color: var(--text);
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 0.7rem 1rem;
  outline: none;
  transition: border-color 0.2s;
  resize: none;
}

.form-group input:focus,
.form-group textarea:focus {
  border-color: var(--pink);
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: #ccc;
}

.btn-send {
  font-family: var(--font-body);
  font-size: 0.9rem;
  font-weight: 500;
  color: #fff;
  background: var(--pink);
  border: none;
  border-radius: 999px;
  padding: 0.85rem;
  cursor: pointer;
  transition: background 0.2s, transform 0.15s;
  margin-top: 0.25rem;
}

.btn-send:hover:not(:disabled) {
  background: var(--pink-dark);
  transform: translateY(-1px);
}

.btn-send:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.sent-state {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
}

.error-msg {
  font-size: 0.8rem;
  color: #e05c5c;
  text-align: center;
  margin-top: -0.5rem;
}

/* ── Footer ── */
.footer {
  border-top: 1px solid var(--border);
  padding: 2rem;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.footer-name {
  font-family: var(--font-display);
  font-size: 1rem;
  font-weight: 600;
  color: var(--text);
}

.footer-copy {
  font-size: 0.78rem;
  color: var(--text-muted);
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .contact {
    padding: 5rem 1.5rem 0;
  }

  .contact-inner {
    flex-direction: column;
     align-items: center; /* ← tambah ini */
    gap: 3rem;
    padding-bottom: 5rem;
  }

  .contact-sub {
    max-width: 100%;
  }

  /* Di mobile, form slide dari bawah bukan dari kanan */
  .anim-right {
    transform: translateY(22px);
  }

  /* Contact links juga dari bawah di mobile */
  .anim-left {
    transform: translateY(22px);
  }
}

/* Reduce motion fallback */
@media (prefers-reduced-motion: reduce) {
  .anim-up, .anim-left, .anim-right {
    transition: none;
    opacity: 1;
    transform: none !important;
  }
}
</style>