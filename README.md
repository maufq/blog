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
4. Haz *commit* y *push* a `main`. Jenkins publicará automáticamente los archivos estáticos.

Para editar la portada, modifica `index.html`. Los estilos compartidos están en `assets/css/styles.css`.

## Configurar Jenkins

1. En Jenkins crea un elemento **Pipeline** o **Multibranch Pipeline** conectado a este repositorio.
2. Configura `Jenkinsfile` como *Script Path* y habilita la construcción de la rama `main`.
3. Crea una credencial de tipo **Secret text** con un token de GitHub que tenga permiso de escritura sobre `maufq/blog`; asígnale exactamente el ID `github-token`.
4. Activa un webhook de GitHub hacia Jenkins para ejecutar el pipeline con cada *push* a `main`, o usa el sondeo periódico si no tienes una URL pública de Jenkins.

El pipeline comprueba que los archivos del sitio existan y después publica únicamente `index.html`, `assets/` y `posts/` en la rama `gh-pages`. GitHub Pages sirve esa rama; `.nojekyll` evita que Jekyll descarte archivos o rutas que puedan añadirse más adelante.

## Publicación manual

Si todavía no has configurado Jenkins, puedes crear la rama de publicación desde un clon local:

```bash
git checkout --orphan gh-pages
git rm -rf .
git checkout main -- index.html assets posts
touch .nojekyll
git add index.html assets posts .nojekyll
git commit -m "Deploy GitHub Pages"
git push origin gh-pages
```
