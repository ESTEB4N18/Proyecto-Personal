# Enciclopedia Interactiva: Destinos Turisticos de Costa Rica

Proyecto web multimedia desarrollado para el curso **IF7102 Multimedios** de la **Universidad de Costa Rica**. La aplicacion presenta una enciclopedia tematica con 10 destinos turisticos costarricenses, busqueda en tiempo real, filtros por categoria, modo claro/oscuro y contenido multimedia.

## Framework utilizado

- Vue 3
- Vite
- Composition API con `<script setup>`
- CSS moderno sin frameworks externos
- Fetch API
- HTML5 Audio

## Instalacion y ejecucion

```bash
npm install
npm run dev
```

Despues de ejecutar el servidor de desarrollo, Vite mostrara la URL local para abrir la aplicacion en el navegador.

## Funcionalidades implementadas

- Enciclopedia interactiva con 10 destinos turisticos de Costa Rica.
- Datos cargados desde `src/data/destinos.json` usando `fetch()` dentro de `onMounted()`.
- Busqueda en tiempo real por nombre de destino y provincia.
- Filtros por categoria: Ciudad, Playa, Montana, Parque Nacional y Volcan.
- Modo claro y oscuro con persistencia en `localStorage`.
- Tarjetas responsive con imagen, nombre, provincia, categoria y boton de detalle.
- Modal con descripcion completa, datos curiosos, cierre con Escape y cierre al hacer clic fuera.
- Audio descriptivo con controles nativos HTML5 para varios destinos.
- Diseno responsive para escritorio, tablet y movil.
- Componentes reutilizables y codigo organizado.

## Capturas sugeridas

- Vista principal en escritorio con tarjetas y hero section.
- Busqueda activa mostrando resultados filtrados.
- Filtro por categoria seleccionado.
- Modal abierto con datos curiosos y reproductor de audio.
- Vista movil.
- Modo oscuro.

## Estructura del proyecto

```text
src/
  components/
    Header.vue
    SearchBar.vue
    CategoryFilter.vue
    DestinationCard.vue
    DestinationModal.vue
    AudioPlayer.vue
    ThemeToggle.vue
  data/
    destinos.json
  assets/
    images/
    audio/
  App.vue
  main.js
  style.css

README.md
REFERENCIAS.md
index.html
package.json
vite.config.js
```

## Recursos multimedia

El JSON usa rutas como `/src/assets/images/monteverde.jpg` y `/src/assets/audio/monteverde.mp3`. Para completar la entrega final, se pueden colocar los archivos multimedia reales en esas carpetas conservando los nombres indicados.
