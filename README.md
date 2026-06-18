# Landing Vue

Landing page ligera en **Vue 3 + Vite + JavaScript + Tailwind v4**, estructurada para
crecer a un catálogo de productos sin reescribir nada.

## Requisitos

- Node.js 18 o superior

## Arrancar el proyecto

```bash
npm install      # instala dependencias (solo la primera vez)
npm run dev      # servidor de desarrollo en http://localhost:5173
npm run build    # genera la versión de producción en /dist
npm run preview  # prueba la build de producción localmente
```

## Estructura

```
src/
├── main.js              # punto de entrada, monta la app y el router
├── App.vue              # layout: header + vista + footer
├── style.css            # Tailwind + tokens de marca (@theme)
├── router/
│   └── index.js         # rutas (el catálogo se carga bajo demanda)
├── views/
│   ├── HomeView.vue     # la landing (hero, producto, previsualizador, contacto)
│   └── CatalogoView.vue # el catálogo
├── components/
│   ├── TheHeader.vue    # navegación responsive
│   ├── TheFooter.vue
│   ├── LogoPreview.vue  # sube un logo y se ve sobre el producto
│   └── ProductCard.vue  # tarjeta reutilizable del catálogo
└── data/
    └── productos.js     # datos de ejemplo (reemplazar por API/CMS después)
```

## Personalizar la marca

Todos los colores salen de `src/style.css`, en el bloque `@theme`.
Cambia `--color-brand` y el resto de la web se recolorea sola. No hay que tocar
nada más para adaptar la paleta a cada cliente.

## Cómo crece a catálogo

- La ruta `/catalogo` ya existe y carga su vista **solo cuando alguien entra ahí**
  (lazy loading), así el landing inicial se mantiene ligero.
- Cuando tengas productos reales, reemplaza `src/data/productos.js` por una llamada
  a tu API. El componente `ProductCard` ya está listo para recibirlos.

## El previsualizador de logo

`LogoPreview.vue` ya funciona: acepta un logo (arrastrar o clic), lo muestra sobre
una maqueta del producto y permite ajustar el tamaño. Para usarlo en serio:

1. Reemplaza el rectángulo gris de la maqueta por una imagen o SVG real de tu producto.
2. Ajusta la posición de la zona del logo según dónde vaya en tu producto.
3. Si quieres permitir descargar la previsualización, se puede renderizar a un
   `<canvas>` y exportar como imagen (siguiente paso natural).
```
