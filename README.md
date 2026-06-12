# Álbum Mundial 2026 🏆

Aplicación web zero-dependency para coleccionar y gestionar las láminas del álbum del Mundial 2026 (México, USA, Canadá).

## Funcionalidades principales

- **Álbum virtual** — 993 láminas distribuidas en 12 grupos (A–L) con 4 países cada uno (20 láminas/país) + sección extras (FWC: 19, Coca-Cola: 14).
- **Registro de repetidas** — Vista independiente para llevar conteo de láminas duplicadas con modal de ajuste (+/-).
- **Persistencia dual** — Guardado automático en JSONBin.io (nube) con fallback a `localStorage`.
- **Protección por contraseña** — Gate inicial con clave `fbb2019`, autenticación persistente en `localStorage`.
- **Búsqueda** — Filtro por nombre de país, grupo, o "otras".
- **Modo "Solo pendientes"** — Oculta países completados en vista álbum.
- **Importar / Exportar** — Respaldo y restauración del progreso en JSON.
- **Compartir** — QR code, copia al portapapeles de texto plano con pendientes y repetidas.
- **Vista pública** — `pendientes.html` (read-only, sin autenticación, sin click handlers).

## Stack

- **Zero dependencias** — HTML + CSS + JS vanilla, sin build tools ni frameworks.
- Dos archivos: `index.html` (app principal) y `pendientes.html` (vista compartible de solo lectura).
- Sin servidor requerido — abre directo en el navegador o con cualquier static server.

## Datos

| Concepto | Cantidad |
|---|---|
| Países | 48 |
| Grupos | 12 (A–L) |
| Láminas por país | 20 |
| Láminas FWC | 19 |
| Láminas Coca-Cola | 14 |
| **Total** | **993** |

## Persistencia

- API: JSONBin.io (`BIN_ID: 6a1b5856ddf5aa59f779a799`)
- Guardado con debounce de 800 ms.
- `localStorage` como fallback (keys: `mundial2026_album`, `mundial2026_duplicates`).

## Uso

```bash
python3 -m http.server 8080
# o simplemente abre index.html en el navegador
```

## Estructura del estado

```js
state = {
  "A_México": Set([1, 3, 5, ...]),     // láminas coleccionadas
  "OTRAS_FWC": Set([2, 10, ...]),
  ...
}

dupState = {
  "A_México": { "1": 2, "5": 1, ... }, // número de lámina → cantidad repetida
  ...
}
```

## Vistas

- **📒 Álbum** — Láminas conseguidas en verde con checkmark; al hacer clic se agregan/eliminan.
- **🔄 Repetidas** — Láminas duplicadas en ámbar; al hacer clic abre modal para ajustar conteo.

## Teclas

- `Escape` — Cierra cualquier modal abierto.
