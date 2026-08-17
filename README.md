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
4. Haz *commit* y *push* a `main`. 

Para editar la portada, modifica `index.html`. Los estilos compartidos están en `assets/css/styles.css`.

## Cómo funcionan los despliegues

Este repositorio utiliza **GitHub Pages** para publicar el sitio de forma automática. No requiere configuración adicional de CI/CD ni herramientas externas.

### Flujo actual

1. **Rama `main`**: contiene los archivos fuente (HTML, CSS, JavaScript)
2. **Rama `gh-pages`**: es la rama que sirve GitHub Pages. Contiene solo los archivos necesarios (`index.html`, `assets/` y `posts/`)
3. **Publicación**: Al hacer *push* a `main`, debes crear/actualizar la rama `gh-pages` con los archivos listos para publicar

### GitHub Actions (opcional)

Actualmente **no hay workflows automáticos configurados**. Los despliegues son manuales. Si quieres automatizar este proceso, puedes:

1. **Opción A - Automatizar con GitHub Actions**: Crear un workflow `.github/workflows/deploy.yml` que actualice automáticamente `gh-pages` con cada *push* a `main`
2. **Opción B - Mantener manual**: Crear `gh-pages` manualmente cada vez que hagas cambios (ver instrucciones más abajo)

### Publicación manual

Para crear o restaurar la rama de publicación manualmente desde un clon local:

```bash
git checkout --orphan gh-pages
git rm -rf .
git checkout main -- index.html assets posts
touch .nojekyll
git add index.html assets posts .nojekyll
git commit -m "Deploy GitHub Pages"
git push origin gh-pages
```

Este comando:
- Crea una rama `gh-pages` sin historial previo
- Extrae solo los archivos necesarios desde `main`
- Añade `.nojekyll` para evitar que Jekyll procese el sitio
- Sube la rama a GitHub para que GitHub Pages la sirva

### Configurar despliegue automático con GitHub Actions

Si prefieres automatizar, puedes crear un workflow similar al `Jenkinsfile` anterior. Crea el archivo `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Validate files
        run: |
          test -f index.html
          test -f assets/css/styles.css
          test -f posts/bienvenido.html
          grep -q '<!doctype html>' index.html
      
      - name: Deploy to gh-pages
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git checkout --orphan gh-pages-temp
          git rm -rf .
          git checkout main -- index.html assets posts
          touch .nojekyll
          git add index.html assets posts .nojekyll
          git commit -m "Deploy GitHub Pages"
          git branch -M gh-pages-temp gh-pages
          git push --force origin gh-pages
```

Este workflow:
- Se ejecuta automáticamente con cada *push* a `main`
- Valida que existan los archivos necesarios
- Actualiza la rama `gh-pages` con los cambios
- No requiere configuración adicional de credenciales (usa el token de GitHub automáticamente)
