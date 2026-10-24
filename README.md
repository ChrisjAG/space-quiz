# Órbita — Quiz de exploración espacial

Órbita es un quiz interactivo en español sobre exploración espacial. Incluye 10 preguntas, feedback explicativo para cada respuesta, puntuación final y soporte para temas claro y oscuro según la preferencia del sistema.

La aplicación es completamente estática y no necesita dependencias, compilación ni servidor de aplicaciones.

## Ejecutar en local

Puedes abrir `index.html` directamente en un navegador. Para probarlo desde un servidor estático local, ejecuta desde la raíz del repositorio:

```powershell
python -m http.server 8000
```

Después abre <http://127.0.0.1:8000/index.html>.

## Publicación

El workflow de GitHub Actions en `.github/workflows/deploy-pages.yml` publica automáticamente el sitio en GitHub Pages cada vez que se actualiza la rama `main`. También puede ejecutarse manualmente desde la pestaña **Actions**.
