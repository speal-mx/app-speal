# SPEAL App — app.speal.mx

App interna de SPEAL (supervisión, operación y administración).

## Cómo está armado

- **Dirección:** `app.speal.mx`. El dominio `speal.mx` está en Neubox; en el editor de zona de cPanel hay un registro **CNAME `app`** que apunta al proyecto de Cloudflare Pages (`<proyecto>.pages.dev`). El correo y la página web no se tocan.
- **Pantalla (este repo):** Cloudflare Pages publica automáticamente lo que hay aquí:
  - `index.html`: pantalla de carga + el puente que recrea `google.script.run` y manda cada llamada al script por `POST`.
  - `config.js`: la URL `/exec` del script.
  - `_headers`, `robots.txt` (no indexar), íconos, `manifest.webmanifest` y `og.jpg` (portada al compartir la liga).
- **Motor:** proyecto de Google Apps Script ligado al Sheets **SPEAL APP OPS** (base de datos). Implementado como aplicación web: *Ejecutar como: yo* y acceso *Cualquier usuario*.
  - `App_API.gs` → `doPost` con la lista de funciones permitidas (`API_PERMITIDAS`) y `getAppHtml`, que entrega la pantalla completa.
  - La entrada es con usuario y contraseña propios de la app.
- **Datos externos:** MASTER DB SPEAL (altas, uniformes), carpetas de Expedientes en Drive, plantilla de Ficha C4.

## Cómo se hacen cambios

- **Cambios de pantalla o de funciones:** se pegan en Apps Script y se publican como **Nueva versión de la misma implementación** (la URL no cambia). La pantalla se descarga del script cada vez que se abre la app, así que **no hay que tocar este repo**.
- **Si se agrega una función nueva del servidor** que llame la pantalla: agregar su nombre en `API_PERMITIDAS` (App_API.gs).
- **Este repo solo se toca** para cambiar la URL `/exec` (`config.js`), los íconos o la portada.
