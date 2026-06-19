<script setup>
import { ref } from 'vue'
import Swal from 'sweetalert2'
import LogoPreview from '@/components/LogoPreview.vue'
import { productos } from '@/data/productos'

const WEBHOOK_URL = import.meta.env.VITE_WEBHOOK_URL

// ── Proceso ──────────────────────────────────────────────────────────────
const pasos = [
  {
    n: '01',
    titulo: 'Envía tu logo',
    desc:   'Sube tus archivos en alta resolución junto con tu paleta de colores corporativos.',
    icon:   'M3 16.5v2.25A2.25 2.25 0 005.25 21h13.5A2.25 2.25 0 0021 18.75V16.5m-13.5-9L12 3m0 0l4.5 4.5M12 3v13.5',
    color:'bg-cyan-500 hover:bg-cyan-600',
  },
  {
    n: '02',
    titulo: 'Elegimos el dulce',
    desc:   'Te asesoramos para seleccionar sabores, formas y texturas según tu público objetivo.',
    icon:   'M9.813 15.904L9 18.75l-.813-2.846a4.5 4.5 0 00-3.09-3.09L2.25 12l2.846-.813a4.5 4.5 0 003.09-3.09L9 5.25l.813 2.846a4.5 4.5 0 003.09 3.09L15.75 12l-2.846.813a4.5 4.5 0 00-3.09 3.09z',
    color:'bg-yellow-500 hover:bg-yellow-600',

  },
  {
    n: '03',
    titulo: 'Vista previa digital',
    desc:   'Aprobamos el diseño juntos antes de arrancar producción. Sin sorpresas.',
    icon:   'M2.036 12.322a1.012 1.012 0 010-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.964-7.178z M15 12a3 3 0 11-6 0 3 3 0 016 0z',
    color:'bg-red-700 hover:bg-red-800',

  },
  {
    n: '04',
    titulo: 'Entrega en tu puerta',
    desc:   'Logística nacional asegurada. Tu pedido llega puntual, listo para servir.',
    icon:   'M8.25 18.75a1.5 1.5 0 01-3 0m3 0a1.5 1.5 0 00-3 0m3 0h6m-9 0H3.375a1.125 1.125 0 01-1.125-1.125V14.25m17.25 4.5a1.5 1.5 0 01-3 0m3 0a1.5 1.5 0 00-3 0m3 0h1.125c.621 0 1.129-.504 1.09-1.124a17.902 17.902 0 00-3.213-9.193 2.056 2.056 0 00-1.58-.86H14.25M16.5 18.75h-2.25m0-11.177v-.958c0-.568-.422-1.048-.987-1.106a48.554 48.554 0 00-10.026 0 1.106 1.106 0 00-.987 1.106v7.635m12-6.677v6.677m0 4.5v-4.5m0 0h-12',
    color:'bg-zinc-700 hover:bg-zinc-800',

  },
]

// ── Sectores ─────────────────────────────────────────────────────────────
const sectores = [
  {
    titulo: 'Hoteles y Restaurantes',
    desc:   'Amenities de cortesía premium para elevar la experiencia del huésped y el comensal. Caramelos y obleas con el logo de tu establecimiento.',
    emoji:  '🍽️',
    badge:  'Mayor demanda',
  },
  {
    titulo: 'Eventos Corporativos',
    desc:   'Regalos de marca memorables para lanzamientos, conferencias y convenciones. Branding comestible que refuerza tu identidad.',
    emoji:  '🏢',
    badge:  null,
  },
  {
    titulo: 'Bodas y Eventos',
    desc:   'Detalles dulces personalizados que tus invitados querrán conservar. Mesas de dulces temáticas y souvenirs comestibles.',
    emoji:  '💍',
    badge:  null,
  },
  {
    titulo: 'Distribuidores',
    desc:   'Precios de mayoreo competitivos y volumen garantizado para mayoristas interesados en productos de alta rotación y calidad artesanal.',
    emoji:  '📦',
    badge:  null,
  },
]

// ── Stats ─────────────────────────────────────────────────────────────────
const stats = [
  { valor: '+500',  label: 'Clientes felices'    },
  { valor: '+2M',   label: 'Dulces entregados'   },
  { valor: '+50',   label: 'Diseños únicos'      },
  { valor: '15',    label: 'Años de experiencia' },
]

// ── Testimonios ───────────────────────────────────────────────────────────
const testimonios = [
  {
    texto:  'La calidad de la impresión en las obleas superó nuestras expectativas. Fue el toque perfecto para nuestro aniversario corporativo.',
    nombre: 'Mariana R.',
    cargo:  'Directora de Marketing',
    empresa:'Hotel Luna',
    inicial:'M',
  },
  {
    texto:  'Confibrinco nos ha apoyado por 3 años consecutivos. La puntualidad y el sabor son su sello distintivo. Nuestros clientes siempre preguntan por los caramelos.',
    nombre: 'Alejandro V.',
    cargo:  'Gerente General',
    empresa:'Bistro Mex',
    inicial:'A',
  },
  {
    texto:  'El proceso de personalización fue muy sencillo. El equipo de diseño nos ayudó a que el logo se viera perfecto en los caramelos. ¡Éxito total!',
    nombre: 'Sofía T.',
    cargo:  'Coordinadora de Eventos',
    empresa:'Eventos Premium',
    inicial:'S',
  },
]

// ── Formulario ────────────────────────────────────────────────────────────
const form = ref({
  nombre:   '',
  email:    '',
  telefono: '',
  empresa:  '',
  producto: '',
  mensaje:  '',
})
const enviado = ref(false)
const enviando = ref(false)

function validar() {
  const errores = []
  const nombre = form.value.nombre.trim()
  const empresa = form.value.empresa.trim()
  const email = form.value.email.trim()
  const telefono = form.value.telefono.trim()
  const producto = form.value.producto
  const mensaje = form.value.mensaje.trim()

  if (nombre.length < 2) errores.push('Ingresa tu nombre completo (mínimo 2 caracteres).')
  if (empresa.length < 2) errores.push('Ingresa el nombre de tu empresa.')
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) errores.push('Ingresa un correo electrónico válido.')
  if (!/^[\d\s+()-]{10,}$/.test(telefono)) errores.push('Ingresa un número de teléfono válido (mínimo 10 dígitos).')
  if (!producto) errores.push('Selecciona el producto que te interesa.')
  if (mensaje.length < 10) errores.push('Cuéntanos un poco más de tu proyecto (mínimo 10 caracteres).')

  return errores
}

async function enviar(e) {
  e.preventDefault()

  const errores = validar()
  if (errores.length) {
    await Swal.fire({
      icon: 'warning',
      title: 'Revisa el formulario',
      html: `<ul style="text-align:left;margin:0;padding-left:1.25rem">${errores.map(x => `<li>${x}</li>`).join('')}</ul>`,
      confirmButtonColor: '#F29F05',
    })
    return
  }

  enviando.value = true
  try {
    const response = await fetch(WEBHOOK_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(form.value),
    })
    if (!response.ok) throw new Error(`HTTP ${response.status}`)

    enviado.value = true
    form.value = { nombre: '', email: '', telefono: '', empresa: '', producto: '', mensaje: '' }
    await Swal.fire({
      icon: 'success',
      title: '¡Solicitud enviada!',
      text: 'Te contactamos en menos de 24 horas.',
      confirmButtonColor: '#F29F05',
    })
  } catch (err) {
    await Swal.fire({
      icon: 'error',
      title: 'No pudimos enviar tu solicitud',
      text: 'Intenta de nuevo o escríbenos a ventas@confibrinco.mx',
      confirmButtonColor: '#D93D04',
    })
  } finally {
    enviando.value = false
  }
}
</script>

<template>

  <!-- ═══════════════════════════════════════════════════════ HERO ══ -->
  <section class="relative overflow-hidden">

    <!-- Imagen decorativa esquina superior izquierda -->
    <img
      src="/elemento.png"
      alt=""
      aria-hidden="true"
      class="pointer-events-none select-none absolute top-0 -right-3 w-48 opacity-80 sm:w-64 sm:top-0 sm:-right-4 lg:w-80"
    />

    <!-- Glow decorativo -->
    <div class="pointer-events-none absolute -top-40 -right-40 h-[500px] w-[500px] rounded-full bg-brand/20 blur-[120px]"></div>
    <div class="pointer-events-none absolute -bottom-20 -left-20 h-80 w-80 rounded-full bg-brand-dark/20 blur-[80px]"></div>

    <div class="relative mx-auto max-w-7xl px-5 py-14 sm:px-6 sm:py-24 lg:py-40">
      <div class="grid gap-10 lg:grid-cols-2 lg:gap-16 items-center">

        <!-- Copy -->
        <div>
          <span class="mb-5 inline-flex items-center gap-2 rounded-full border border-brand/30 bg-brand/10 px-4 py-1.5 text-sm font-medium text-brand">
            <span class="h-1.5 w-1.5 rounded-full bg-brand animate-pulse"></span>
            Aliado Estratégico B2B
          </span>

          <h1 class="text-[2.6rem] font-extrabold leading-[1.08] tracking-tight text-gray-900 sm:text-5xl lg:text-7xl">
           <span  class="text-brand">Dulces</span>  que<br/>
            llevan tu<br/>
            <span class="text-brand">Marca</span> a<br/>
            <span class="text-brand"> otro nivel</span>
          </h1>

          <p class="mt-5 text-base text-muted max-w-md leading-relaxed sm:text-lg">
            Suministro premium de dulces personalizados para restaurantes, hoteles,
            bodas y eventos corporativos. Con el logotipo de tu negocio impreso en cada pieza.
          </p>

          <div class="mt-8 flex flex-col gap-3 sm:flex-row sm:flex-wrap sm:gap-4">
            <a
              href="/#contacto"
              class="rounded-2xl bg-brand px-7 py-4 text-center text-sm font-bold text-white transition hover:bg-brand-dark shadow-lg shadow-brand/30 active:scale-95"
            >
              Solicitar cotización
            </a>
            <a
              href="/#catalogo"
              class="rounded-2xl border border-brand/30 px-7 py-4 text-center text-sm font-semibold text-ink transition hover:bg-brand/10 active:scale-95"
            >
              Ver catálogo →
            </a>
          </div>

          <!-- Mini-stats -->
          <div class="mt-10 grid grid-cols-3 gap-4 border-t border-ink/10 pt-8 sm:mt-14 sm:gap-6 sm:pt-10">
            <div>
              <p class="text-xl font-bold text-ink sm:text-2xl">5,000<span class="text-brand">+</span></p>
              <p class="mt-1 text-[10px] text-muted uppercase tracking-wider sm:text-xs">piezas / día</p>
            </div>
            <div>
              <p class="text-xl font-bold text-ink sm:text-2xl">120<span class="text-brand">+</span></p>
              <p class="mt-1 text-[10px] text-muted uppercase tracking-wider sm:text-xs">sabores</p>
            </div>
            <div>
              <p class="text-xl font-bold text-ink sm:text-2xl">24<span class="text-brand">h</span></p>
              <p class="mt-1 text-[10px] text-muted uppercase tracking-wider sm:text-xs">respuesta</p>
            </div>
          </div>
        </div>

        <!-- Mobile: product preview cards 2x2 -->
        <div class="grid grid-cols-2 gap-3 lg:hidden">
          <div
            v-for="p in productos"
            :key="p.id"
            class="group relative overflow-hidden rounded-2xl bg-brand-soft p-5 ring-1 ring-black/5"
          >
            <div class="text-5xl mb-3">{{ p.emoji }}</div>
            <p class="font-bold text-ink text-xs leading-tight">{{ p.nombre }}</p>
            <p class="text-[10px] text-muted mt-0.5">{{ p.subtitulo }}</p>
            <span class="mt-2 inline-block rounded-full bg-brand/15 px-2 py-0.5 text-[9px] font-semibold text-brand uppercase tracking-wider">
              Personalizable
            </span>
          </div>
        </div>

        <!-- Desktop: candy grid -->
        <div class="hidden lg:grid grid-cols-2 gap-4">
          <div
            v-for="p in productos"
            :key="p.id"
            class="group relative overflow-hidden rounded-3xl bg-brand-soft p-7 ring-1 ring-black/5 transition hover:ring-brand/30"
          >
            <div class="absolute inset-0 bg-gradient-to-br opacity-0 group-hover:opacity-10 transition-opacity" :class="p.color"></div>
            <div class="text-6xl mb-4">{{ p.emoji }}</div>
            <p class="font-bold text-ink text-sm">{{ p.nombre }}</p>
            <p class="text-xs text-muted mt-1">{{ p.subtitulo }}</p>
            <span class="mt-3 inline-block rounded-full bg-brand/10 px-2.5 py-0.5 text-[10px] font-semibold text-brand uppercase tracking-wider">
              Personalizable
            </span>
          </div>
        </div>

      </div>
    </div>
  </section>


  <!-- ═══════════════════════════════════════ CATÁLOGO ══ -->
  <section id="catalogo" class="relative overflow-hidden py-16 bg-brand-soft sm:py-24">

    <!-- Imagen decorativa esquina superior derecha -->
    <img
      src="/elemento.png"
      alt=""
      aria-hidden="true"
      class="pointer-events-none select-none absolute top-3 -right-2 w-16 sm:w-32 sm:top-10 sm:right-5 lg:w-44 opacity-80"
    />

    <div class="relative mx-auto max-w-7xl px-5 sm:px-6">

      <div class="mx-auto mb-10 max-w-2xl text-center sm:mb-14">
        <span class="mb-3 inline-block text-sm font-semibold uppercase tracking-widest text-brand">Nuestro catálogo</span>
        <h2 class="text-2xl font-bold tracking-tight text-ink sm:text-3xl sm:text-4xl">
          Formatos listos para recibir<br/>
          <span class="text-brand">tu identidad visual</span>
        </h2>
        <p class="mt-4 text-sm text-muted sm:text-base">
          Cada producto existe en decenas de sabores, formas y tamaños.
          Todos personalizables con el logo de tu restaurante o empresa.
        </p>
      </div>

      <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
        <article
          v-for="p in productos"
          :key="p.id"
          class="group relative overflow-hidden rounded-2xl bg-white ring-1 ring-black/5 shadow-sm transition hover:shadow-xl hover:-translate-y-1 sm:rounded-3xl"
        >
          <!-- Color banner -->
          <div
            class="h-64 grid place-items-center overflow-hidden md:h-64"
            :class="p.image ? 'bg-white' : ['bg-gradient-to-br', p.color]"
          >
            <img v-if="p.image" :src="p.image" :alt="p.nombre" class="h-full w-full object-contain p-6" />
            <span v-else class="text-6xl sm:text-7xl">{{ p.emoji }}</span>
          </div>

          <!-- Badge -->
          <span class="absolute top-2.5 right-2.5 rounded-full bg-ink/80 px-2 py-0.5 text-[9px] font-bold uppercase tracking-wider text-white backdrop-blur sm:top-3 sm:right-3 sm:text-[10px] sm:px-3 sm:py-1">
            Personalizable
          </span>

          <div class="p-4 sm:p-6">
            <h3 class="font-bold text-sm text-ink sm:text-lg">{{ p.nombre }}</h3>
            <p class="mt-1 text-[10px] font-medium text-brand uppercase tracking-wider sm:text-xs">{{ p.subtitulo }}</p>
            <p class="mt-2 text-xs text-muted leading-relaxed hidden sm:block">{{ p.descripcion }}</p>
            <a
              href="/#contacto"
              class="mt-3 inline-flex items-center gap-1 text-xs font-semibold text-brand hover:underline sm:mt-5 sm:text-sm"
            >
              Ver detalles →
            </a>
          </div>
        </article>
      </div>

    </div>
  </section>


  <!-- ═════════════════════════════════════ LOGO PREVIEW ══ -->
  <LogoPreview />


  <!-- ════════════════════════════════════════════ PROCESO ══ -->
  <section id="proceso" class="relative overflow-hidden py-16 bg-white sm:py-24">

    <!-- Imagen decorativa esquina inferior izquierda -->
    <img
      src="/elemento.png"
      alt=""
      aria-hidden="true"
      class="pointer-events-none select-none absolute -bottom-8 -left-8 w-32 opacity-80 sm:w-44 sm:-bottom-10 sm:-left-10 lg:w-56"
    />

    <div class="relative mx-auto max-w-7xl px-5 sm:px-6">

      <div class="mx-auto mb-12 max-w-2xl text-center sm:mb-16">
        <span class="mb-3 inline-block text-sm font-semibold uppercase tracking-widest text-brand">Así trabajamos</span>
        <h2 class="text-2xl font-bold tracking-tight text-ink sm:text-3xl sm:text-4xl">
          Tu marca, <span class="text-brand">hecha dulce</span>
        </h2>
        <p class="mt-4 text-sm text-muted sm:text-base">
          Un proceso sencillo y transparente, de cuatro pasos, desde tu archivo hasta la mesa de tu restaurante.
        </p>
      </div>

      <!-- Steps: single col on mobile, 2-col on sm, 4-col on lg -->
      <div class="relative grid gap-6 sm:grid-cols-2 lg:grid-cols-4 lg:gap-8">

        <!-- Línea conectora desktop -->
        <div class="absolute top-10 left-0 right-0 hidden h-px bg-brand/20 lg:block" style="margin: 0 12.5%;"></div>

        <div
          v-for="paso in pasos"
          :key="paso.n"
          class="relative flex items-start gap-5 rounded-2xl bg-brand-soft p-5 sm:flex-col sm:items-center sm:text-center sm:bg-transparent sm:p-0 sm:gap-0"
         
        >
          <!-- Step number -->
          <div class="relative z-10 flex-none flex h-16 w-16 flex-col items-center justify-center rounded-full text-white shadow-lg sm:mb-6 sm:h-20 sm:w-20" :class="paso.color">
            <span class="text-[10px] font-bold opacity-60">PASO</span>
            <span class="text-xl font-extrabold leading-none sm:text-2xl">{{ paso.n }}</span>
          </div>

          <div>
            <h3 class="font-bold text-ink mb-1 sm:mb-2">{{ paso.titulo }}</h3>
            <p class="text-sm text-muted leading-relaxed">{{ paso.desc }}</p>
          </div>
        </div>
      </div>

    </div>
  </section>


  <!-- ═══════════════════════════════════════════ SECTORES ══ -->
  <section
    id="sectores"
    class="relative overflow-hidden py-16 sm:py-24"
    style="background: linear-gradient(135deg, #FFF7E6 0%, #FFE8C2 50%, #FFD9A8 100%)"
  >

    <!-- Imagen decorativa esquina superior derecha -->
    <img
      src="/elemento.png"
      alt=""
      aria-hidden="true"
      class="pointer-events-none select-none absolute -top-12 -right-12 w-40 opacity-80 sm:w-56 sm:-top-16 sm:-right-16 lg:w-72"
    />

    <div class="relative mx-auto max-w-7xl px-5 sm:px-6">

      <div class="mx-auto mb-10 max-w-2xl text-center sm:mb-14">
        <span class="mb-3 inline-block text-sm font-semibold uppercase tracking-widest text-brand">Soluciones a medida</span>
        <h2 class="text-2xl font-bold tracking-tight text-ink sm:text-3xl sm:text-4xl">
          ¿Para quién <span class="text-brand">endulzamos?</span>
        </h2>
        <p class="mt-4 text-sm text-muted sm:text-base">
          Trabajamos con cada sector de una manera distinta, adaptando el producto, el volumen y la presentación.
        </p>
      </div>

      <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4 sm:gap-5">
        <div
          v-for="s in sectores"
          :key="s.titulo"
          class="group relative overflow-hidden rounded-2xl bg-white p-6 ring-1 ring-black/5 shadow-sm transition hover:ring-brand/40 hover:shadow-md sm:rounded-3xl sm:p-8"
        >
          <!-- Badge "Mayor demanda" -->
          <span
            v-if="s.badge"
            class="absolute top-3 right-3 rounded-full bg-brand px-2.5 py-0.5 text-[10px] font-bold uppercase tracking-wider text-white sm:top-4 sm:right-4"
          >
            {{ s.badge }}
          </span>

          <!-- Mobile: horizontal layout -->
          <div class="flex items-start gap-4 sm:block">
            <div class="text-4xl shrink-0 sm:text-5xl sm:mb-5">{{ s.emoji }}</div>
            <div>
              <h3 class="font-bold text-ink mb-2 sm:mb-3">{{ s.titulo }}</h3>
              <p class="text-sm text-muted leading-relaxed">{{ s.desc }}</p>
            </div>
          </div>

          <a
            href="/#contacto"
            class="mt-4 inline-flex items-center gap-1 text-sm font-semibold text-brand hover:underline sm:mt-6"
          >
            Ver soluciones →
          </a>
        </div>
      </div>

    </div>
  </section>


  <!-- ══════════════════════════════════════════════ STATS ══ -->
  <section class="relative overflow-hidden py-16 sm:py-20" style="background: linear-gradient(135deg, #F29F05 0%, #F28705 50%, #D93D04 100%)">

    <!-- Imagen decorativa esquina superior izquierda -->
    <img
      src="/elemento.png"
      alt=""
      aria-hidden="true"
      class="pointer-events-none select-none absolute -top-6 -left-6 w-24 opacity-80 sm:w-32 sm:-top-8 sm:-left-8 lg:w-40"
    />

    <div class="relative mx-auto max-w-7xl px-5 sm:px-6">
      <div class="grid grid-cols-2 gap-8 lg:grid-cols-4 text-center">
        <div v-for="s in stats" :key="s.label">
          <p class="text-4xl font-extrabold text-white sm:text-5xl">{{ s.valor }}</p>
          <p class="mt-2 text-xs font-medium text-white/80 uppercase tracking-wider sm:text-sm">{{ s.label }}</p>
        </div>
      </div>
    </div>
  </section>


  <!-- ══════════════════════════════════════ TESTIMONIOS ══ -->
  <section id="testimonios" class="relative overflow-hidden py-16 bg-brand-soft sm:py-24">

    <!-- Imagen decorativa esquina inferior derecha -->
    <img
      src="/elemento.png"
      alt=""
      aria-hidden="true"
      class="pointer-events-none select-none absolute -bottom-10 -right-10 w-36 opacity-80 sm:w-52 sm:-bottom-14 sm:-right-14 lg:w-64"
    />

    <div class="relative mx-auto max-w-7xl px-5 sm:px-6">

      <div class="mx-auto mb-10 max-w-2xl text-center sm:mb-14">
        <span class="mb-3 inline-block text-sm font-semibold uppercase tracking-widest text-brand">Lo que dicen de nosotros</span>
        <h2 class="text-2xl font-bold tracking-tight text-ink sm:text-3xl sm:text-4xl">
          Marcas que ya <span class="text-brand">confían en nosotros</span>
        </h2>
      </div>

      <!-- Mobile: horizontal scroll; desktop: grid -->
      <div class="flex gap-5 overflow-x-auto snap-x snap-mandatory pb-4 -mx-5 px-5 sm:mx-0 sm:px-0 sm:overflow-visible sm:grid sm:grid-cols-3 sm:gap-6">
        <blockquote
          v-for="t in testimonios"
          :key="t.nombre"
          class="flex-none w-[82vw] snap-start flex flex-col rounded-2xl bg-white p-6 ring-1 ring-black/5 shadow-sm sm:w-auto sm:rounded-3xl sm:p-8"
        >
          <!-- Stars -->
          <div class="flex gap-0.5 mb-4">
            <svg v-for="i in 5" :key="i" class="h-4 w-4 fill-brand" viewBox="0 0 20 20">
              <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"/>
            </svg>
          </div>

          <p class="flex-1 text-sm text-muted leading-relaxed italic">"{{ t.texto }}"</p>

          <div class="mt-5 flex items-center gap-3">
            <div class="grid h-10 w-10 shrink-0 place-items-center rounded-full bg-brand text-sm font-bold text-white">
              {{ t.inicial }}
            </div>
            <div>
              <p class="text-sm font-semibold text-ink">{{ t.nombre }}</p>
              <p class="text-xs text-muted">{{ t.cargo }} · {{ t.empresa }}</p>
            </div>
          </div>
        </blockquote>
      </div>

      <!-- Scroll indicator for mobile -->
      <div class="flex justify-center gap-1.5 mt-4 sm:hidden">
        <span v-for="i in testimonios.length" :key="i" class="h-1.5 w-6 rounded-full bg-brand/30 first:bg-brand"></span>
      </div>

    </div>
  </section>


  <!-- ═══════════════════════════════════════════ CONTACTO ══ -->
  <section id="contacto" class="relative overflow-hidden py-16  sm:py-24">

    <!-- Imagen decorativa esquina inferior izquierda -->
    <img
      src="/elemento.png"
      alt=""
      aria-hidden="true"
      class="pointer-events-none select-none absolute -bottom-12 -left-12 w-40 opacity-80 sm:w-56 sm:-bottom-16 sm:-left-16 lg:w-72"
    />

    <div class="relative mx-auto max-w-7xl px-5 sm:px-6">
      <div class="grid gap-12 lg:grid-cols-2 lg:gap-16 items-start">

        <!-- Info contacto -->
        <div>
          <span class="mb-3 inline-block text-sm font-semibold uppercase tracking-widest text-brand">Comencemos</span>
          <h2 class="text-2xl font-bold tracking-tight text-ink sm:text-3xl sm:text-4xl">
            ¡Creemos algo <span class="text-brand">dulce juntos!</span>
          </h2>
          <p class="mt-4 text-sm text-muted leading-relaxed max-w-md sm:mt-5 sm:text-base">
            Nuestro equipo de diseño te asesora sin costo. Cuéntanos tu necesidad
            y en menos de <strong class="text-ink">24 horas</strong> tendrás
            una propuesta con muestras digitales de tu logo en nuestros dulces.
          </p>

          <ul class="mt-8 space-y-4 text-sm sm:mt-10 sm:space-y-5">
            <li class="flex items-center gap-3 text-muted">
              <span class="grid h-10 w-10 shrink-0 place-items-center rounded-xl bg-brand/10 text-brand">
                <svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 6.75c0 8.284 6.716 15 15 15h2.25a2.25 2.25 0 002.25-2.25v-1.372c0-.516-.351-.966-.852-1.091l-4.423-1.106c-.44-.11-.902.055-1.173.417l-.97 1.293c-.282.376-.769.542-1.21.38a12.035 12.035 0 01-7.143-7.143c-.162-.441.004-.928.38-1.21l1.293-.97c.363-.271.527-.734.417-1.173L6.963 3.102a1.125 1.125 0 00-1.091-.852H4.5A2.25 2.25 0 002.25 4.5v2.25z"/>
                </svg>
              </span>
              <div>
                <p class="font-medium text-ink">Ventas & WhatsApp</p>
                <p>+52 33 1234 5678</p>
              </div>
            </li>
            <li class="flex items-center gap-3 text-muted">
              <span class="grid h-10 w-10 shrink-0 place-items-center rounded-xl bg-brand/10 text-brand">
                <svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M21.75 6.75v10.5a2.25 2.25 0 01-2.25 2.25h-15a2.25 2.25 0 01-2.25-2.25V6.75m19.5 0A2.25 2.25 0 0019.5 4.5h-15a2.25 2.25 0 00-2.25 2.25m19.5 0v.243a2.25 2.25 0 01-1.07 1.916l-7.5 4.615a2.25 2.25 0 01-2.36 0L3.32 8.91a2.25 2.25 0 01-1.07-1.916V6.75"/>
                </svg>
              </span>
              <div>
                <p class="font-medium text-ink">Correo electrónico</p>
                <p>ventas@confibrinco.mx</p>
              </div>
            </li>
            <li class="flex items-center gap-3 text-muted">
              <span class="grid h-10 w-10 shrink-0 place-items-center rounded-xl bg-brand/10 text-brand">
                <svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M15 10.5a3 3 0 11-6 0 3 3 0 016 0z"/>
                  <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 10.5c0 7.142-7.5 11.25-7.5 11.25S4.5 17.642 4.5 10.5a7.5 7.5 0 1115 0z"/>
                </svg>
              </span>
              <div>
                <p class="font-medium text-ink">Showroom & Fábrica</p>
                <p>Etzatlan, Jalisco, México</p>
              </div>
            </li>
          </ul>
        </div>

        <!-- Formulario -->
        <div class="rounded-2xl bg-brand-soft p-6 ring-1 ring-black/5 sm:rounded-3xl sm:p-8">

          <form @submit="enviar" novaldate class="flex flex-col gap-4 sm:gap-5">
            <div class="grid gap-4 sm:grid-cols-2 sm:gap-5">
              <div>
                <label class="mb-1.5 block text-xs font-semibold uppercase tracking-wider text-muted">Nombre completo</label>
                <input
                  v-model="form.nombre"
                  type="text"
                  required
                  placeholder="Ej. Juan Pérez"
                  class="w-full rounded-xl bg-white px-4 py-3.5 text-sm text-ink placeholder-muted/60 ring-1 ring-black/10 focus:outline-none focus:ring-brand"
                />
              </div>
              <div>
                <label class="mb-1.5 block text-xs font-semibold uppercase tracking-wider text-muted">Empresa</label>
                <input
                  v-model="form.empresa"
                  type="text"
                  required
                  placeholder="Nombre de tu negocio"
                  class="w-full rounded-xl bg-white px-4 py-3.5 text-sm text-ink placeholder-muted/60 ring-1 ring-black/10 focus:outline-none focus:ring-brand"
                />
              </div>
            </div>

            <div class="grid gap-4 sm:grid-cols-2 sm:gap-5">
              <div>
                <label class="mb-1.5 block text-xs font-semibold uppercase tracking-wider text-muted">Correo corporativo</label>
                <input
                  v-model="form.email"
                  type="email"
                  required
                  placeholder="juan@restaurante.com"
                  class="w-full rounded-xl bg-white px-4 py-3.5 text-sm text-ink placeholder-muted/60 ring-1 ring-black/10 focus:outline-none focus:ring-brand"
                />
              </div>
              <div>
                <label class="mb-1.5 block text-xs font-semibold uppercase tracking-wider text-muted">Teléfono</label>
                <input
                  v-model="form.telefono"
                  type="tel"
                  required
                  inputmode="tel"
                  autocomplete="tel"
                  placeholder="Numero de telefono"
                  class="w-full rounded-xl bg-white px-4 py-3.5 text-sm text-ink placeholder-muted/60 ring-1 ring-black/10 focus:outline-none focus:ring-brand"
                />
              </div>
            </div>
            <div>
              <label class="mb-1.5 block text-xs font-semibold uppercase tracking-wider text-muted">¿Qué producto te interesa?</label>
              <select
                v-model="form.producto"
                class="w-full rounded-xl bg-white px-4 py-3.5 text-sm text-ink ring-1 ring-black/10 focus:outline-none focus:ring-brand appearance-none"
              >
                <option value="" disabled>Selecciona un producto</option>
                <option value="caramelos">Caramelos</option>
                <option value="gomitas">Gomitas</option>
                <option value="paletas">Paletas</option>
                <option value="obleas">Obleas</option>
                <option value="combo">Combinación / no sé aún</option>
              </select>
            </div>

            <div>
              <label class="mb-1.5 block text-xs font-semibold uppercase tracking-wider text-muted">Cuéntanos tu proyecto</label>
              <textarea
                v-model="form.mensaje"
                rows="4"
                placeholder="Cantidad aproximada, tipo de evento o negocio, fecha de entrega…"
                class="w-full rounded-xl bg-white px-4 py-3.5 text-sm text-ink placeholder-muted/60 ring-1 ring-black/10 focus:outline-none focus:ring-brand resize-none"
              ></textarea>
            </div>

            <button
              type="submit"
              :disabled="enviando"
              class="w-full rounded-xl py-4 text-sm font-bold text-white transition active:scale-95 shadow-lg shadow-brand/20 disabled:opacity-60 disabled:cursor-not-allowed"
              style="background: linear-gradient(135deg, #F29F05, #D93D04)"
            >
              {{ enviando ? 'Enviando…' : 'Enviar solicitud de cotización' }}
            </button>

            <p class="text-center text-xs text-muted">
              Te respondemos en menos de 24 h · Sin compromiso
            </p>
          </form>
        </div>

      </div>
    </div>
  </section>

</template>
