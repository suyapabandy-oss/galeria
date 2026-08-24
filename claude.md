# Contexto del Proyecto: Galería Suyapa Monterroso

Hola Claude. Estás hablando con Suyapa Monterroso (o asistiéndola directamente). Este repositorio es su galería de arte personal alojada en GitHub Pages (dominio: suyapamonterroso.com). Anteriormente estaba en WordPress, pero ha sido migrada a un sitio estático moderno.

## Arquitectura del Sitio
- **Frontend Público:** `index.html`, CSS, Vanilla JS. Lee los datos dinámicamente desde `obras.json`.
- **Base de Datos:** `obras.json` actúa como una base de datos NoSQL.
- **Backend / Admin:** `admin.html` es una Progressive Web App (PWA) diseñada para el iPhone de Suyapa. Utiliza la **API REST de GitHub** para hacer commits directos:
  1. Convierte la foto tomada con el iPhone a Base64.
  2. Sube la foto a la carpeta `img/obras/`.
  3. Actualiza el archivo `obras.json` añadiendo la nueva ficha técnica.
- **PWA:** `manifest.json` permite instalar la app en iOS.

## Tareas Pendientes para ti (Claude)
1. **Configurar API de GitHub en admin.html:** El código actual de `admin.html` es un esqueleto. Debes implementar la lógica JS (`fetch` a `https://api.github.com/repos/suyapabandy-oss/galeria/contents/...`) usando un Personal Access Token (PAT).
2. *Seguridad:* Asegúrate de implementar una forma segura para que el PAT resida en el navegador de Suyapa (ej. `localStorage`) sin exponerlo en el código público.
3. **Manejo de CSS/UI:** Mantén un diseño elegante (fuentes Cormorant Garamond y Montserrat, colores terrosos #9c7b50 y #faf8f5).
4. **Asistencia a Suyapa:** Explícale los pasos técnicos con mucha paciencia, claridad y sin jerga compleja. Ella usará esta herramienta desde Safari en su iPhone.
