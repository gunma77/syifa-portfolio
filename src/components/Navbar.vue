<template>
  <nav class="navbar" :class="{ scrolled: isScrolled }">
    <div class="nav-inner">
      <ul class="nav-links">
        <li v-for="link in links" :key="link.href">
          <a :href="link.href">{{ link.label }}</a>
        </li>
      </ul>

      <a href="#contact" class="nav-cta">Let's Work Together</a>

      <!-- Mobile hamburger -->
      <button class="hamburger" @click="menuOpen = !menuOpen" aria-label="Toggle menu">
        <span></span><span></span><span></span>
      </button>
    </div>

    <!-- Mobile menu -->
    <div class="mobile-menu" :class="{ open: menuOpen }">
      <a v-for="link in links" :key="link.href" :href="link.href" @click="menuOpen = false">
        {{ link.label }}
      </a>
      <a href="#contact" class="nav-cta-mobile" @click="menuOpen = false">Let's Work Together</a>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isScrolled = ref(false)
const menuOpen = ref(false)

const links = [
  { href: '#about',   label: 'About' },
  { href: '#skills',  label: 'Skills' },
  { href: '#experience', label: 'Experience' },
  { href: '#projects', label: 'Projects' },
]

function onScroll() {
  isScrolled.value = window.scrollY > 20
}

onMounted(() => window.addEventListener('scroll', onScroll))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  height: var(--nav-h);
  transition: background 0.3s, box-shadow 0.3s;
}

.navbar.scrolled {
  background: rgba(253, 250, 249, 0.92);
  backdrop-filter: blur(12px);
  box-shadow: 0 1px 0 var(--border);
}

.nav-inner {
  max-width: 1100px;
  margin: 0 auto;
  padding: 0 2rem;
  height: 100%;
  display: flex;
  align-items: center;
  gap: 2rem;
  justify-content: center;
}

.nav-logo {
  font-family: var(--font-display);
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--text);
  text-decoration: none;
  margin-right: auto;
}

.nav-logo span {
  color: var(--pink);
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 2rem;
}

.nav-links a {
  font-size: 0.875rem;
  font-weight: 400;
  color: var(--text-muted);
  text-decoration: none;
  letter-spacing: 0.02em;
  transition: color 0.2s;
}

.nav-links a:hover {
  color: var(--text);
}

.nav-cta {
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--pink-dark);
  text-decoration: none;
  border: 1px solid var(--pink);
  padding: 0.45rem 1.1rem;
  border-radius: 999px;
  white-space: nowrap;
  transition: background 0.2s, color 0.2s;
}

.nav-cta:hover {
  background: var(--pink);
  color: #fff;
}

/* Hamburger */
.hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}

.hamburger span {
  display: block;
  width: 22px;
  height: 2px;
  background: var(--text);
  border-radius: 2px;
  transition: 0.2s;
}

/* Mobile menu */
.mobile-menu {
  display: none;
  flex-direction: column;
  background: rgba(253, 250, 249, 0.97);
  padding: 1rem 2rem 1.5rem;
  gap: 1rem;
  border-top: 1px solid var(--border);
}

.mobile-menu a {
  font-size: 1rem;
  color: var(--text);
  text-decoration: none;
}

.nav-cta-mobile {
  display: inline-block;
  color: var(--pink-dark) !important;
  font-weight: 500;
  border: 1px solid var(--pink);
  padding: 0.5rem 1rem;
  border-radius: 999px;
  width: fit-content;
}

@media (max-width: 768px) {
  .nav-links,
  .nav-cta {
    display: none;
  }

  .hamburger {
    display: flex;
  }

  .mobile-menu.open {
    display: flex;
  }
}
</style>