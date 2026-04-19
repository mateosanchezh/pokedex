# Pokédex Angular

[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
[![codecov](https://codecov.io/gh/keilermora/pokedex-angular/branch/master/graph/badge.svg?token=9E0D28IOFT)](https://codecov.io/gh/keilermora/pokedex-angular)
[![Security Headers](https://img.shields.io/badge/Security%20Headers-A%2B-brightgreen)](https://securityheaders.com/?q=https://black-mushroom-07502f110.7.azurestaticapps.net/)

**Producción:** [https://black-mushroom-07502f110.7.azurestaticapps.net/](https://black-mushroom-07502f110.7.azurestaticapps.net/)

La aplicación muestra el listado y el detalle de los Pokémon de las primeras 3 generaciones.

La imagen que representa un Pokémon en el listado muestra las variaciones que estos tuvieron durante las primeras versiones, desde la versión Green (1996) hasta la versión Emerald (2005).

Los detalles de un Pokémon individual muestra sus estadísticas base y los registros de la Pokédex de las diferentes versiones.

El proyecto fue desarrollado usando [Angular](https://angular.io/) para la interfaz de usuario, en comunicación con la API GraphQL de [PokéAPI](https://pokeapi.co/).

## Requisitos mínimos

- [Node.js](https://nodejs.org) LTS
- Un navegador web

## Desarrollo local

```bash
npm install
npm start       # http://localhost:4200
npm test        # Ejecutar pruebas unitarias
npm run lint    # Verificar estilo de código
```

## Despliegue

La aplicación está desplegada en **Azure Static Web Apps** con CI/CD automático via GitHub Actions. Cada push a `main` genera un nuevo despliegue.

Los encabezados HTTP de seguridad están configurados en `staticwebapp.config.json`, logrando una calificación **A+** en [securityheaders.com](https://securityheaders.com/).

Para más detalles del proceso de despliegue, ver [DESPLIEGUE.md](DESPLIEGUE.md).

## Tecnologías

- [Angular](https://angular.io/) — Framework principal
- [Apollo Client](https://www.apollographql.com/docs/angular/) — Cliente GraphQL
- [PokéAPI](https://pokeapi.co/) — Fuente de datos
- [Azure Static Web Apps](https://azure.microsoft.com/es-es/products/app-service/static) — Hosting en la nube
- [GitHub Actions](https://github.com/features/actions) — CI/CD
- [Font Awesome](https://fontawesome.com/) — Iconos
- [Normalize.css](https://necolas.github.io/normalize.css/) — Reset de estilos
