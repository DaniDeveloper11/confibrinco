<script setup>
import { ref, computed } from 'vue'

const logoSrc  = ref(null)
const logoName = ref('')
const size     = ref(38)
const dragOver = ref(false)

function handleFile(file) {
  if (!file || !file.type.startsWith('image/')) return
  logoName.value = file.name
  const reader = new FileReader()
  reader.onload = (e) => { logoSrc.value = e.target.result }
  reader.readAsDataURL(file)
}

function onInputChange(e) { handleFile(e.target.files?.[0]) }
function onDrop(e) { dragOver.value = false; handleFile(e.dataTransfer?.files?.[0]) }
function reset() { logoSrc.value = null; logoName.value = ''; size.value = 38 }

const logoStyle = computed(() => ({ width: `${size.value}%` }))
</script>

<template>
  <section id="previsualizar" class="py-24 bg-brand-soft">
    <div class="mx-auto max-w-7xl px-6">

      <div class="mx-auto mb-14 max-w-2xl text-center">
        <span class="mb-3 inline-block rounded-full bg-brand/10 px-4 py-1 text-sm font-semibold text-brand">
          Previsualizacion de envoltura
        </span>
        <h2 class="text-3xl font-bold tracking-tight sm:text-4xl text-ink">
          Mira tu logo en nuestros dulces
        </h2>
        <p class="mt-4 text-muted">
          Sube el logo de tu restaurante o evento y observa cómo quedaría impreso.
          Sin instalar nada, en segundos.
        </p>
      </div>

      <div class="grid gap-8 lg:grid-cols-2 items-center">

        <!-- Controles -->
        <div class="flex flex-col justify-center">
          <label
            class="flex cursor-pointer flex-col items-center justify-center rounded-2xl border-2 border-dashed p-10 text-center transition"
            :class="dragOver
              ? 'border-brand bg-brand/5'
              : 'border-brand/30 bg-white hover:border-brand'"
            @dragover.prevent="dragOver = true"
            @dragleave.prevent="dragOver = false"
            @drop.prevent="onDrop"
          >
            <svg width="44" height="44" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" class="mb-3 text-brand">
              <path d="M12 16V4m0 0L8 8m4-4l4 4" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M4 16v2a2 2 0 002 2h12a2 2 0 002-2v-2" stroke-linecap="round"/>
            </svg>
            <span class="font-semibold text-ink">Arrastra tu logo o haz clic aquí</span>
            <span class="mt-1 text-sm text-muted">PNG, JPG o SVG · alta resolución</span>
            <input type="file" accept="image/*" class="sr-only" @change="onInputChange"/>
          </label>

          <div v-if="logoSrc" class="mt-6 rounded-2xl bg-white p-5 ring-1 ring-brand/20">
            <div class="flex items-center justify-between mb-4">
              <span class="truncate text-sm text-muted">{{ logoName }}</span>
              <button class="text-sm font-medium text-brand hover:underline shrink-0 ml-3" @click="reset">Quitar</button>
            </div>
            <label class="block text-sm font-medium text-ink">
              Tamaño del logo
              <input v-model="size" type="range" min="15" max="70" class="mt-2 w-full accent-[var(--color-brand)]"/>
            </label>
          </div>

          <p class="mt-5 text-xs text-muted text-center">
            Vista previa digital · Aprobamos el diseño antes de fabricar.
          </p>
        </div>

        <!-- Maqueta del producto -->
        <div class="grid place-items-center rounded-3xl bg-white p-8 shadow-lg ring-1 ring-brand/10">
          <div class="relative w-full max-w-sm">
            <!-- Imagen base del caramelo -->
            <img src="/caramelo_base.png" alt="Empaque base" class="w-full" />

            <!-- Logo superpuesto centrado -->
            <div class="absolute inset-0 flex items-center justify-center">
              <img
                v-if="logoSrc"
                :src="logoSrc"
                :style="logoStyle"
                alt="Vista previa del logo"
                class="object-contain drop-shadow-sm transition-all"
              />
              <span v-else class="text-xs text-muted/50 italic text-center px-6">Tu logo aparecerá aquí</span>
            </div>

            <!-- Badge maqueta -->
            <span class="absolute bottom-3 left-3 rounded-lg bg-white/90 px-2.5 py-1 text-xs font-medium text-muted backdrop-blur shadow">
              Maqueta de empaque
            </span>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>
