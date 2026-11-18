# Viaje al Templo de Hermosillo 2026

Sitio informativo del viaje de juventud de la Estaca Culiacán México Humaya.

## Stack

- Vue 3
- Vite
- Bootstrap 5
- Bootstrap Icons

## Requisitos

Vite actual requiere Node.js 20.19+ o 22.12+.

## Desarrollo local

```bash
npm install
npm run dev
```

Vite mostrará una URL local, normalmente `http://localhost:5173`.

## Build de producción

```bash
npm run build
npm run preview
```

El build se genera en `dist/`.

## Subir a Git

```bash
git init
git add .
git commit -m "Sitio inicial viaje al Templo de Hermosillo"
git branch -M main
git remote add origin <URL-DE-TU-REPOSITORIO>
git push -u origin main
```

## Publicar en Vercel desde Git

1. Crea el repositorio en GitHub, GitLab o Bitbucket y sube este proyecto.
2. En Vercel selecciona **Add New > Project**.
3. Importa el repositorio.
4. Vercel debe detectar **Vite** automáticamente.
5. Build command: `npm run build`.
6. Output directory: `dist`.
7. Pulsa **Deploy**.

Después de vincular Git, los nuevos pushes producen despliegues automáticamente. La rama de producción (normalmente `main`) actualiza producción y otras ramas pueden generar previews.

## Contenido

La información principal está centralizada en `src/data/tripData.js` para facilitar cambios en grupos, talleres y rol de Trivia.

Las infografías están en `public/` y se referencian desde la app con rutas como `/grupo-1.png`.
