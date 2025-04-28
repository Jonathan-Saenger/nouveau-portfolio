<template>
  <nav class="navbar fixed w-full z-50">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <!-- Logo/Nom -->
        <div class="flex-shrink-0">
          <h1 class="hidden md:block text-xl font-semibold">Jonathan Saenger</h1>
          <span class="md:hidden text-2xl font-bold">JS</span>
        </div>

        <!-- Menu Navigation -->
        <div class="flex items-center space-x-4">
          <!-- Version Desktop -->
          <div class="hidden md:flex items-center space-x-4">
            <RouterLink
              to="/"
              class="nav-link px-3 py-2 rounded-md text-sm font-medium"
              :class="{ 'active': $route.path === '/' }"
            >
              Qui suis-je
            </RouterLink>
            <RouterLink
              to="/parcours"
              class="nav-link px-3 py-2 rounded-md text-sm font-medium"
              :class="{ 'active': $route.path === '/parcours' }"
            >
              Mon parcours
            </RouterLink>
            <RouterLink
              to="/contact"
              class="nav-link px-3 py-2 rounded-md text-sm font-medium"
              :class="{ 'active': $route.path === '/contact' }"
            >
              Contact
            </RouterLink>
          </div>

          <!-- Version Mobile avec icônes -->
          <div class="flex md:hidden items-center space-x-4">
            <RouterLink
              to="/"
              class="nav-link p-2"
              :class="{ 'active': $route.path === '/' }"
              aria-label="Qui suis-je"
            >
              <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
              </svg>
            </RouterLink>
            <RouterLink
              to="/parcours"
              class="nav-link p-2"
              :class="{ 'active': $route.path === '/parcours' }"
              aria-label="Mon parcours"
            >
              <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3" />
              </svg>
            </RouterLink>
            <RouterLink
              to="/contact"
              class="nav-link p-2"
              :class="{ 'active': $route.path === '/contact' }"
              aria-label="Contact"
            >
              <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
              </svg>
            </RouterLink>
          </div>

          <!-- Toggle Mode Sombre -->
          <button
            @click="toggleDarkMode"
            class="dark-mode-toggle p-2 rounded-lg"
            aria-label="Toggle dark mode"
          >
            <svg
              v-if="isDarkMode"
              class="w-5 h-5"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
            </svg>
            <svg
              v-else
              class="w-5 h-5"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const isDarkMode = ref(false);

const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;
  // Sauvegarder la préférence dans localStorage
  localStorage.setItem('darkMode', isDarkMode.value ? 'true' : 'false');
  applyTheme(isDarkMode.value);
};

const applyTheme = (isDark: boolean) => {
  if (isDark) {
    document.documentElement.setAttribute('data-theme', 'dark');
    document.documentElement.classList.add('dark');
  } else {
    document.documentElement.removeAttribute('data-theme');
    document.documentElement.classList.remove('dark');
  }
};

onMounted(() => {
  // Vérifier d'abord la préférence sauvegardée
  const savedTheme = localStorage.getItem('darkMode');
  if (savedTheme) {
    isDarkMode.value = savedTheme === 'true';
  } else {
    // Si pas de préférence sauvegardée, utiliser la préférence système
    isDarkMode.value = window.matchMedia('(prefers-color-scheme: dark)').matches;
  }

  // Appliquer le thème
  applyTheme(isDarkMode.value);

  // Écouter les changements de préférence système
  window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
    if (!localStorage.getItem('darkMode')) {
      isDarkMode.value = e.matches;
      applyTheme(e.matches);
    }
  });
});
</script>