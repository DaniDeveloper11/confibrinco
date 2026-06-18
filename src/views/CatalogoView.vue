<script setup>
import ProductCard from '@/components/ProductCard.vue'
import { productos } from '@/data/productos.js'
</script>

<template>
  <section class="py-20">
    <div class="mx-auto max-w-6xl px-6">
      <div class="mb-12 max-w-2xl">
        <h1 class="text-3xl font-bold tracking-tight sm:text-4xl">Catálogo</h1>
        <p class="mt-4 text-muted">
          Aquí irán tus productos. Por ahora se cargan desde un archivo de ejemplo;
          más adelante puedes conectarlo a tu API o CMS sin tocar el resto de la web.
        </p>
      </div>

      <div class="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
        <ProductCard
          v-for="producto in productos"
          :key="producto.id"
          :producto="producto"
        />
      </div>
    </div>
  </section>
</template>
