<script setup>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const open = ref(false)

const links = [
  { label: 'Catálogo',        href: '/#catalogo'    },
  { label: 'Cómo funciona',   href: '/#proceso'     },
  { label: 'Sectores',        href: '/#sectores'    },
  { label: 'Testimonios',     href: '/#testimonios' },
]
</script>

<template>
  <header class="sticky top-0 z-50 bg-white/96  backdrop-blur border-b border-brand/15 md:border-white/5 shadow-sm md:shadow-none">
    <nav class="mx-auto flex max-w-7xl items-center justify-between px-4 py-2.5 md:px-6 md:py-4">

      <!-- Logo -->
      <RouterLink to="/" class="flex items-center shrink-0">
        <img
          src="/logo_confibrinco.png"
          alt="Confibrinco"
          class="h-11 w-auto md:h-10 drop-shadow-sm"
        />
      </RouterLink>

      <!-- Desktop nav -->
      <div class="hidden md:flex items-center gap-8">
        <a
          v-for="l in links"
          :key="l.label"
          :href="l.href"
          class="text-sm text-brand-dark transition hover:text-white"
        >
          {{ l.label }}
        </a>
      </div>

      <!-- CTA desktop -->
      <a
        href="/#contacto"
        class="hidden md:inline-flex items-center gap-2 rounded-xl bg-brand px-5 py-2.5 text-sm font-semibold text-white transition hover:bg-brand-dark"
      >
        Solicitar cotización
      </a>

      <!-- Hamburger -->
      <button
        class="md:hidden text-ink/70 hover:text-ink transition p-2 -mr-1 rounded-xl hover:bg-brand/10"
        :aria-expanded="open"
        aria-label="Abrir menú"
        @click="open = !open"
      >
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
          <path v-if="!open" d="M4 6h16M4 12h16M4 18h16" stroke-linecap="round"/>
          <path v-else d="M6 6l12 12M6 18L18 6" stroke-linecap="round"/>
        </svg>
      </button>
    </nav>

    <!-- Mobile drawer -->
    <Transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="opacity-0 -translate-y-2"
      enter-to-class="opacity-100 translate-y-0"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="opacity-100 translate-y-0"
      leave-to-class="opacity-0 -translate-y-2"
    >
      <div v-if="open" class="md:hidden border-t border-brand/20 overflow-hidden" style="background: linear-gradient(160deg, #F29F05 0%, #F28705 60%, #D93D04 100%)">
        <div class="px-5 py-5 flex flex-col gap-1">
          <a
            v-for="l in links"
            :key="l.label"
            :href="l.href"
            class="flex items-center justify-between text-[15px] font-semibold text-white py-3.5 border-b border-white/20 last:border-0 active:opacity-70"
            @click="open = false"
          >
            {{ l.label }}
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" class="opacity-60">
              <path d="M9 18l6-6-6-6" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </a>
          <a
            href="/#contacto"
            class="mt-3 rounded-2xl bg-ink px-5 py-4 text-center text-[15px] font-bold text-white shadow-lg active:opacity-80"
            @click="open = false"
          >
            Solicitar cotización →
          </a>
          <p class="mt-3 text-center text-xs text-white/60">Respuesta en menos de 24 h</p>
        </div>
      </div>
    </Transition>
  </header>
</template>
