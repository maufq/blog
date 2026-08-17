# Blog

Blog estático, sin dependencias, diseñado para publicarse con GitHub Pages.

## Publicación inicial

El repositorio ya queda preparado para que GitHub Pages sirva la rama `gh-pages`.
Tras el primer despliegue, estará disponible en:

<https://maufq.github.io/blog/>

## Escribir un artículo

1. Copia `posts/bienvenido.html` y ponle un nombre URL amigable, por ejemplo `posts/mi-articulo.html`.
2. Cambia su título, fecha, descripción y contenido.
3. Añade una tarjeta que apunte al nuevo archivo dentro de la sección `#articulos` de `index.html`.
4. Haz *commit* y *push* a `main`. Los cambios se publicarán automáticamente en GitHub Pages.

Para editar la portada, modifica `index.html`. Los estilos compartidos están en `assets/css/styles.css`.

## Cómo funciona la publicación

GitHub Pages publica automáticamente los archivos de la rama `gh-pages`. La rama se actualiza automáticamente con cada *push* a `main`, y solo incluye los archivos necesarios: `index.html`, `assets/` y `posts/`. El archivo `.nojekyll` evita que Jekyll descarte archivos o rutas que puedan añadirse más adelante.

## Publicación manual

Si necesitas crear o restaurar la rama de publicación manualmente desde un clon local:

```bash
git checkout --orphan gh-pages
git rm -rf .
git checkout main -- index.html assets posts
touch .nojekyll
git add index.html assets posts .nojekyll
git commit -m "Deploy GitHub Pages"
git push origin gh-pages
```
