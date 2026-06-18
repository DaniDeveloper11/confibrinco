<script setup>
const year = new Date().getFullYear()

const nav = [
  { label: 'Catálogo',       href: '/#catalogo'    },
  { label: 'Cómo funciona',  href: '/#proceso'     },
  { label: 'Sectores',       href: '/#sectores'    },
  { label: 'Testimonios',    href: '/#testimonios' },
  { label: 'Cotizar',        href: '/#contacto'    },
]
</script>

<template>
  <footer class="bg-dark text-white/60">
    <div class="mx-auto max-w-7xl px-5 py-12 sm:px-6 sm:py-16">
      <div class="grid gap-10 sm:grid-cols-2 lg:grid-cols-4">

        <!-- Brand col -->
        <div class="lg:col-span-2">
          <a href="/" class="inline-flex items-center mb-5">
            <img
              src="/logo_confibrinco.png"
              alt="Confibrinco"
              class="h-12 w-auto drop-shadow-md"
            />
          </a>
          <p class="text-sm leading-relaxed max-w-xs">
            Fábrica artesanal de dulces personalizados. Combinamos tradición dulcera mexicana
            con innovación para crear branding comestible de clase mundial.
          </p>
          <p class="mt-5 text-sm">
            <span class="text-white font-medium">+52 33 1234 5678</span><br/>
            ventas@confibrinco.mx<br/>
            Etzatlan, Jalisco, México
          </p>
        </div>

        <!-- Nav col -->
        <div>
          <h3 class="mb-4 text-xs font-semibold uppercase tracking-widest text-white/30">Explora</h3>
          <ul class="space-y-3 text-sm">
            <li v-for="l in nav" :key="l.label">
              <a :href="l.href" class="transition hover:text-white">{{ l.label }}</a>
            </li>
          </ul>
        </div>

        <!-- Legal col -->
        <div>
          <h3 class="mb-4 text-xs font-semibold uppercase tracking-widest text-white/30">Legal</h3>
          <ul class="space-y-3 text-sm">
            <li><a href="#" class="transition hover:text-white">Aviso de privacidad</a></li>
            <li><a href="#" class="transition hover:text-white">Términos de servicio</a></li>
          </ul>
        </div>
      </div>

      <div class="mt-10 border-t border-white/10 pt-8 flex flex-col gap-2 text-xs sm:flex-row sm:items-center sm:justify-between sm:gap-4 sm:mt-12">
        <p>© {{ year }} Confibrinco Artisanal Sugarcrafts. Todos los derechos reservados.</p>
        <p>Hecho con 🍬 en Etzatlan, Jalisco</p>
      </div>
    </div>
  </footer>
</template>
