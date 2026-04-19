# Pokedex Angular - Despliegue en Azure Static Web Apps

## Descripción del Proyecto

Este proyecto consiste en el despliegue de una aplicación web llamada **Pokedex**, desarrollada en **Angular**, la cual permite visualizar información de diferentes Pokémon consumiendo datos desde una API externa.

El objetivo principal fue desplegar la aplicación en la nube utilizando **Azure Static Web Apps**, aplicar buenas prácticas de seguridad web y documentar el proceso completo.

---

## Creación de Cuenta en Azure

Para realizar el despliegue se utilizó el portal de Microsoft Azure.

Pasos realizados:

1. Se accedió al portal de Azure mediante la dirección:

https://portal.azure.com

2. Se inició sesión con una cuenta Microsoft.
3. Se verificó que la suscripción estuviera activa.
4. Se ingresó al servicio **Static Web Apps** desde el buscador del portal.

---

## Repositorio Utilizado

Se utilizó un repositorio en GitHub con el siguiente enlace:

https://github.com/mateosanchezh/pokedex

Este repositorio contiene el código fuente completo de la aplicación desarrollada en Angular.

---

## Objetivo del Despliegue

Los objetivos principales fueron:

- Publicar la aplicación en una URL pública.
- Implementar integración continua (CI/CD).
- Aplicar encabezados HTTP de seguridad.
- Verificar el nivel de seguridad del sitio web.

---

## Seguridad Implementada

Se configuraron encabezados HTTP de seguridad mediante el archivo:

staticwebapp.config.json

Los encabezados implementados fueron:

- Content-Security-Policy
- Strict-Transport-Security
- X-Content-Type-Options
- X-Frame-Options
- Referrer-Policy
- Permissions-Policy

Estos encabezados permitieron mejorar la seguridad del sitio web.

---

## URL Pública de la Aplicación

La aplicación desplegada se encuentra disponible en:

https://black-mushroom-07502f110.7.azurestaticapps.net/

---

## Resultado del Escaneo de Seguridad

Se realizó un análisis de seguridad utilizando la herramienta:

https://securityheaders.com/

Resultado obtenido:

A+

Esto indica que los encabezados de seguridad fueron configurados correctamente.

---

## Tecnologías Utilizadas

Las tecnologías utilizadas en el proyecto fueron:

- Angular
- TypeScript
- HTML
- CSS
- Azure Static Web Apps
- GitHub
- GitHub Actions
- SecurityHeaders

---

## Conclusión

Durante este laboratorio se aprendió el proceso completo de despliegue de aplicaciones web en la nube utilizando Azure Static Web Apps, incluyendo la implementación de seguridad mediante encabezados HTTP.

Se logró publicar la aplicación correctamente, automatizar el despliegue y obtener una calificación A+ en seguridad web.
