<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const sidebarOpen = ref(false)

// close sidebar on ESC
function onKey(e: KeyboardEvent) {
  if (e.key === 'Escape') sidebarOpen.value = false
}
onMounted(() => window.addEventListener('keydown', onKey))
onBeforeUnmount(() => window.removeEventListener('keydown', onKey))

const overlayVisible = computed(() => sidebarOpen.value)

const nav = [
  { label: 'Dashboard', icon: 'M3 12h18M3 6h18M3 18h18' },
  { label: 'Projects', icon: 'M4 6h16v12H4z' },
  { label: 'Teams', icon: 'M5 20h14M12 14a4 4 0 1 0 0-8 4 4 0 0 0 0 8z' },
  { label: 'Settings', icon: 'M12 15.5a3.5 3.5 0 1 0 0-7 3.5 3.5 0 0 0 0 7z' },
]
</script>

<template>
  <div class="app">
    <!-- Mobile overlay -->
    <div
      v-if="overlayVisible"
      class="overlay"
      @click="sidebarOpen = false"
      aria-hidden="true"
    />

    <!-- Sidebar -->
    <aside
      class="sidebar"
      :class="{ 'sidebar--open': sidebarOpen }"
      aria-label="Sidebar Navigation"
    >
      <div class="sidebar__brand">
        <svg viewBox="0 0 24 24" class="brand__icon" aria-hidden="true">
          <circle cx="12" cy="12" r="10" />
          <path d="M12 6v6l4 2" />
        </svg>
        <span class="brand__name">YourBrand</span>
      </div>

      <nav class="sidebar__nav">
        <button
          v-for="item in nav"
          :key="item.label"
          class="nav__item"
          :aria-label="item.label"
        >
          <svg viewBox="0 0 24 24" class="nav__icon" aria-hidden="true">
            <path :d="item.icon" stroke-linecap="round" stroke-linejoin="round" />
          </svg>
          <span>{{ item.label }}</span>
        </button>
      </nav>

      <div class="sidebar__footer">
        <button class="btn btn--ghost">Log out</button>
      </div>
    </aside>

    <!-- Main column -->
    <div class="main">
      <!-- Header / Topbar -->
      <header class="topbar">
        <button
          class="icon-btn show-on-mobile"
          aria-label="Open sidebar"
          @click="sidebarOpen = true"
        >
          <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
            <path d="M3 6h18M3 12h18M3 18h18" stroke-linecap="round" />
          </svg>
        </button>

        <div class="topbar__search">
          <input type="search" placeholder="Search…" aria-label="Search" />
        </div>

        <div class="topbar__actions">
          <button class="icon-btn" aria-label="Notifications">
            <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
              <path d="M15 17h5l-1.4-1.4A2 2 0 0 1 18 14.2V11a6 6 0 1 0-12 0v3.2c0 .5-.2 1-.6 1.4L4 17h5" />
              <path d="M9 17a3 3 0 0 0 6 0" />
            </svg>
          </button>
          <div class="avatar" aria-label="User menu" role="img">G</div>
        </div>
      </header>

      <!-- Breadcrumb / Page header -->
      <div class="page-head">
        <h1>Page Title</h1>
        <div class="page-head__actions">
          <button class="btn">Primary</button>
          <button class="btn btn--ghost">Secondary</button>
        </div>
      </div>

      <!-- Content -->
      <main class="content">
        <slot>
          <!-- Demo cards -->
          <section class="grid">
            <article class="card">
              <h3>Card One</h3>
              <p>Use this area for your components/content.</p>
              <button class="btn btn--sm">Action</button>
            </article>
            <article class="card">
              <h3>Card Two</h3>
              <p>Drop in tables, charts, whatever.</p>
              <button class="btn btn--sm">Action</button>
            </article>
            <article class="card">
              <h3>Card Three</h3>
              <p>Responsive out of the box.</p>
              <button class="btn btn--sm">Action</button>
            </article>
          </section>
        </slot>
      </main>

      <!-- Footer -->
      <footer class="footer">
        <span>© {{ new Date().getFullYear() }} YourBrand</span>
        <span>Built with Vue 3</span>
      </footer>
    </div>
  </div>
</template>

<style scoped>
/* ====== Base ====== */
:root {
  --bg: #0b0c10;
  --panel: #111218;
  --muted: #1a1b22;
  --text: #e8e8e8;
  --text-dim: #b8b8c2;
  --primary: #6c5ce7;
  --primary-600: #5a49e0;
  --ring: #35374a;
  --border: #23242d;
  --shadow: 0 10px 30px rgba(0,0,0,0.35);
}

* { box-sizing: border-box; }
html, body, :host { height: 100%; }
body { margin: 0; background: var(--bg); color: var(--text); font-family: ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji", "Segoe UI Emoji"; }

/* ====== Layout Grid ====== */
.app {
  display: grid;
  grid-template-columns: 280px 1fr;
  grid-template-rows: 100vh;
  background: var(--bg);
}

/* Mobile: sidebar overlays */
@media (max-width: 1024px) {
  .app { grid-template-columns: 1fr; }
}

/* ====== Sidebar ====== */
.sidebar {
  position: sticky;
  top: 0;
  height: 100vh;
  background: var(--panel);
  border-right: 1px solid var(--border);
  box-shadow: var(--shadow);
  display: flex;
  flex-direction: column;
  padding: 16px;
  gap: 12px;
  z-index: 40;
}

@media (max-width: 1024px) {
  .sidebar {
    position: fixed;
    left: -300px;
    width: 280px;
    transition: left 160ms ease;
  }
  .sidebar--open { left: 0; }
  .overlay {
    position: fixed; inset: 0; background: rgba(0,0,0,.5);
    backdrop-filter: blur(2px);
    z-index: 30;
  }
}

.sidebar__brand {
  display: flex; align-items: center; gap: 10px; padding: 8px;
}
.brand__icon {
  width: 28px; height: 28px; fill: none; stroke: var(--primary); stroke-width: 2;
}
.brand__name { font-weight: 700; letter-spacing: .2px; }

.sidebar__nav { display: grid; gap: 6px; margin-top: 8px; }
.nav__item {
  display: flex; align-items: center; gap: 10px;
  width: 100%; padding: 10px 12px;
  background: transparent; border: 1px solid transparent; color: var(--text);
  text-align: left; border-radius: 12px; cursor: pointer;
}
.nav__item:hover { background: var(--muted); border-color: var(--border); }
.nav__icon { width: 20px; height: 20px; fill: none; stroke: currentColor; stroke-width: 1.8; }

.sidebar__footer { margin-top: auto; padding-top: 12px; border-top: 1px dashed var(--border); }

/* ====== Main Column ====== */
.main {
  display: grid;
  grid-template-rows: auto auto 1fr auto;
  min-height: 100vh;
}

/* Topbar */
.topbar {
  position: sticky; top: 0; z-index: 20;
  display: grid; grid-template-columns: auto 1fr auto;
  align-items: center; gap: 12px;
  padding: 12px 16px; background: rgba(11,12,16,0.7);
  backdrop-filter: saturate(120%) blur(8px);
  border-bottom: 1px solid var(--border);
}
.topbar__search { display: flex; align-items: center; }
.topbar__search input {
  width: 100%; max-width: 520px;
  background: var(--panel); color: var(--text);
  border: 1px solid var(--border);
  border-radius: 12px; padding: 10px 12px; outline: none;
}
.topbar__search input:focus { border-color: var(--ring); box-shadow: 0 0 0 3px rgba(108,92,231,.2); }

.topbar__actions { display: flex; align-items: center; gap: 10px; }

.show-on-mobile { display: none; }
@media (max-width: 1024px) {
  .show-on-mobile { display: inline-flex; }
}

/* Page head */
.page-head {
  display: flex; align-items: center; justify-content: space-between;
  gap: 12px; padding: 16px; border-bottom: 1px solid var(--border);
}
.page-head h1 { margin: 0; font-size: 1.25rem; }
.page-head__actions { display: flex; gap: 10px; flex-wrap: wrap; }

/* Content */
.content { padding: 16px; }
.grid {
  display: grid; gap: 16px;
  grid-template-columns: repeat(12, 1fr);
}
.card {
  grid-column: span 12;
  background: var(--panel); border: 1px solid var(--border);
  border-radius: 16px; padding: 16px; box-shadow: var(--shadow);
}
.card h3 { margin: 0 0 8px; }

@media (min-width: 640px) {
  .card { grid-column: span 6; }
}
@media (min-width: 1024px) {
  .card { grid-column: span 4; }
}

/* Footer */
.footer {
  display: flex; align-items: center; justify-content: space-between;
  padding: 14px 16px; border-top: 1px solid var(--border); color: var(--text-dim);
}

/* ====== Buttons & Bits ====== */
.btn {
  appearance: none; border: 1px solid var(--primary-600);
  background: var(--primary); color: white;
  padding: 10px 14px; border-radius: 12px; font-weight: 600;
  cursor: pointer; transition: transform .06s ease, filter .2s ease;
}
.btn:hover { filter: brightness(1.08); }
.btn:active { transform: translateY(1px); }
.btn--ghost {
  background: transparent; color: var(--text); border: 1px solid var(--border);
}
.btn--sm { padding: 8px 12px; font-size: .9rem; }

.icon-btn {
  display: inline-grid; place-items: center;
  width: 38px; height: 38px; border-radius: 12px;
  background: var(--panel); border: 1px solid var(--border);
  cursor: pointer;
}
.icon { width: 22px; height: 22px; fill: none; stroke: var(--text); stroke-width: 2; }

.avatar {
  display: grid; place-items: center;
  width: 36px; height: 36px; border-radius: 50%;
  background: linear-gradient(135deg, var(--primary), var(--primary-600));
  color: white; font-weight: 700;
}
</style>