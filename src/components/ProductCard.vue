<script setup>
defineProps({
  producto: {
    type: Object,
    required: true,
  },
})
</script>

<template>
  <article class="group overflow-hidden rounded-2xl bg-white ring-1 ring-slate-100 transition hover:shadow-lg">
    <div class="aspect-[4/3] bg-gradient-to-br from-slate-100 to-slate-200">
      <!-- Reemplaza por <img :src="producto.imagen" /> cuando tengas fotos -->
      <div class="grid h-full place-items-center text-sm text-muted">
        {{ producto.nombre }}
      </div>
    </div>
    <div class="p-5">
      <h3 class="font-semibold">{{ producto.nombre }}</h3>
      <p class="mt-1 text-sm text-muted">{{ producto.descripcion }}</p>
      <p v-if="producto.precio" class="mt-3 font-semibold text-brand">{{ producto.precio }}</p>
    </div>
  </article>
</template>
