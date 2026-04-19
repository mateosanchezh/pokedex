# Proceso de Despliegue en Azure Static Web Apps

Documento del proceso realizado para desplegar la aplicación **Pokedex Angular** en Azure Static Web Apps, incluyendo configuración, seguridad y problemas encontrados.

---

## Paso 1: Acceso al Portal de Azure

Se accedió a [https://portal.azure.com](https://portal.azure.com) con una cuenta Microsoft y se verificó que la suscripción estuviera activa. Desde el buscador del portal se ingresó al servicio **Static Web Apps**.

---

## Paso 2: Creación de la Static Web App

Se creó el recurso con la siguiente configuración:

| Parámetro | Valor |
|---|---|
| Grupo de Recursos | `rg-pokedex-prod` |
| Nombre | `swa-pokedex-portal-prod-YS` |
| Plan | Free |
| Región | Central US |

---

## Paso 3: Conexión con GitHub

Se vinculó el repositorio [https://github.com/mateosanchezh/pokedex](https://github.com/mateosanchezh/pokedex) con la siguiente configuración:

| Campo | Valor |
|---|---|
| Branch | `main` |
| Build Preset | Angular |
| App location | `/` |
| API location | *(vacío)* |
| Output location | `dist/pokedex-angular` |

> El valor de **Output location** se obtuvo del campo `outputPath` en `angular.json`.

Azure generó automáticamente un workflow de GitHub Actions en `.github/workflows/`.

---

## Paso 4: Verificación del Despliegue

Después de 2 a 5 minutos, la aplicación quedó disponible en:

**[https://black-mushroom-07502f110.7.azurestaticapps.net/](https://black-mushroom-07502f110.7.azurestaticapps.net/)**

Se verificó que la aplicación cargara correctamente, sin errores y con HTTPS activo.

![SSL Certificate](https://i.imgur.com/oRIfdqX.png)

---

## Paso 5: Implementación de Encabezados de Seguridad

Se creó el archivo `staticwebapp.config.json` en la raíz del proyecto con los siguientes encabezados HTTP:

```json
{
  "globalHeaders": {
    "Content-Security-Policy": "...",
    "Strict-Transport-Security": "max-age=31536000; includeSubDomains",
    "X-Content-Type-Options": "nosniff",
    "X-Frame-Options": "DENY",
    "Referrer-Policy": "no-referrer",
    "Permissions-Policy": "geolocation=(), camera=(), microphone=()"
  }
}
```

El cambio se desplegó con:

```bash
git add staticwebapp.config.json
git commit -m "Agregar headers de seguridad"
git push origin main
```

Azure detectó el push y realizó un redeploy automático.

---

## Paso 6: Problemas Encontrados y Soluciones

### Problema 1 — CSP bloqueaba estilos inline de Angular

**Error en consola:**
```
Applying inline style violates the following Content Security Policy directive
'default-src 'self''. Either the 'unsafe-inline' keyword or a hash is required.
```

**Causa:** El `Content-Security-Policy` inicial no tenía una directiva `style-src` explícita, por lo que caía al fallback de `default-src` que no permitía estilos inline. Angular inyecta estilos inline para los componentes, por lo que la aplicación no renderizaba correctamente.

**Solución:** Se agregó la directiva `style-src` con `'unsafe-inline'` de forma explícita:
```
style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
```

---

### Problema 2 — CSP con `'unsafe-inline'` en `script-src` bajaba la calificación de seguridad

**Síntoma:** La herramienta [securityheaders.com](https://securityheaders.com/) reportaba calificación **A** en lugar de **A+** con el mensaje:
> *"This policy contains 'unsafe-inline' which is dangerous in the script-src directive."*

**Causa:** El `script-src` tenía `'unsafe-inline'` configurado por defecto, lo cual es innecesario para Angular en producción (los scripts se compilan como archivos `.js` externos).

**Solución:** Se eliminó `'unsafe-inline'` de `script-src`, quedando solo `'self'`:
```
script-src 'self';
```

---

### Problema 3 — Imágenes de versión no cargaban en Azure

**Síntoma:** Los sprites de versión (pixel art) en las tarjetas de Pokémon no se mostraban en el despliegue de Azure, aunque sí funcionaban en desarrollo local.

**Causa:** El archivo `environment.prod.ts` tenía `imagesPath: '/pokedex-angular/assets/images'`, una ruta configurada para GitHub Pages donde la aplicación se sirve desde `/pokedex-angular/`. En Azure la aplicación se sirve desde la raíz `/`, por lo que esa ruta resultaba en un 404.

**Solución:** Se actualizó `environment.prod.ts`:
```typescript
// Antes (GitHub Pages)
imagesPath: '/pokedex-angular/assets/images'

// Después (Azure)
imagesPath: '/assets/images'
```

---

## Paso 7: Resultado de Seguridad

Se verificó la seguridad del sitio con [https://securityheaders.com/](https://securityheaders.com/):

**Calificación obtenida: A+**

![Security Headers A+](https://i.imgur.com/B6iZVj7.png)

Encabezados implementados correctamente:

- `Content-Security-Policy` — Controla recursos permitidos (scripts, estilos, imágenes, fuentes, conexiones)
- `Strict-Transport-Security` — Fuerza el uso de HTTPS
- `X-Content-Type-Options` — Previene MIME-sniffing
- `X-Frame-Options` — Bloquea el uso del sitio en iframes (anti-clickjacking)
- `Referrer-Policy` — Controla la información enviada en cabecera Referer
- `Permissions-Policy` — Desactiva acceso a geolocalización, cámara y micrófono

---

## Paso 8: Limitación — Dominio Personalizado

Se intentó configurar un dominio personalizado para la aplicación. Azure Static Web Apps requiere un registro **CNAME** para validar y asociar dominios personalizados.

El proveedor de dominio gratuito utilizado (**DuckDNS**) no soporta registros CNAME, únicamente registros tipo A. Por esta razón **no fue posible configurar un dominio personalizado** y la aplicación permanece en el dominio generado por Azure:

`https://black-mushroom-07502f110.7.azurestaticapps.net/`

> Para configurar un dominio personalizado en el futuro se requiere un proveedor DNS que soporte CNAME (por ejemplo: Cloudflare, Namecheap, Google Domains).

---

## Resultado Final

| Aspecto | Estado |
|---|---|
| Despliegue | Exitoso |
| HTTPS | Activo |
| CI/CD (GitHub Actions) | Configurado |
| Calificación de seguridad | A+ |
| Dominio personalizado | No disponible (limitación DuckDNS) |
