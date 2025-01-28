# MrvVuej
![image](https://github.com/user-attachments/assets/b16da2dd-ab02-481e-9922-59f418f608f6)

Este proyecto es el reto Front-end de desarrollo con VUE

## Requisitos

- Node.js (versión 14 o superior)
- npm (versión 6 o superior)

## Instrucciones para instalar las dependencias

Para instalar las dependencias del proyecto, ejecuta el siguiente comando en la raíz del proyecto:

```sh
npm install
```

## Configuración y ejecución del proyecto

### Compilar y recargar en caliente para desarrollo

Para iniciar el servidor de desarrollo con recarga en caliente, ejecuta:

```sh
npm run dev
```

### Compilar y minificar para producción

Para compilar el proyecto para producción, ejecuta:

```sh
npm run build
```

### Previsualizar la compilación de producción

Para previsualizar la compilación de producción, ejecuta:

```sh
npm run preview
```

### Ejecutar pruebas unitarias con Vitest

Para ejecutar las pruebas unitarias, ejecuta:

```sh
npm run test:unit
```

### Ejecutar pruebas de extremo a extremo con Cypress

Para ejecutar las pruebas de extremo a extremo en el servidor de desarrollo, ejecuta:

```sh
npm run test:e2e:dev
```

Para ejecutar las pruebas de extremo a extremo en la compilación de producción, ejecuta:

```sh
npm run build
npm run test:e2e
```

## Estructura del proyecto

La estructura del proyecto es la siguiente:

```
.gitignore
.vscode/
  extensions.json
  settings.json
cypress/
  e2e/
    example.cy.js
  fixtures/
    example.json
  jsconfig.json
  support/
    commands.js
    e2e.js
cypress.config.js
index.html
jsconfig.json
package.json
public/
README.md
src/
  App.vue
  assets/
    base.css
    main.css
  components/
    UserList.vue
  main.js
  services/
    api.js
vite.config.js
vitest.config.js
```

### Descripción de los archivos y directorios principales

- **.gitignore**: Archivos y directorios que Git debe ignorar.
- **.vscode**: Configuración específica de Visual Studio Code.
- **cypress**: Archivos de configuración y pruebas de Cypress.
- **cypress.config.js**: Configuración de Cypress.
- **index.html**: Archivo HTML principal.
- **jsconfig.json**: Configuración de JavaScript.
- **package.json**: Información del proyecto y scripts de npm.
- **public**: Archivos públicos.
- **README.md**: Documentación del proyecto.
- **src**: Código fuente del proyecto.
  - **App.vue**: Componente principal de la aplicación.
  - `assets/`: Archivos CSS y otros recursos.
  - `components/`: Componentes Vue reutilizables.
    - **UserList.vue**: Componente para la lista de usuarios.
  - **main.js**: Punto de entrada de la aplicación.
  - `services/`: Servicios de la aplicación.
    - **api.js**: Configuración de Axios para llamadas a la API.
- **vite.config.js**: Configuración de Vite.
- **vitest.config.js**: Configuración de Vitest.

---
