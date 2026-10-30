---
title: "Lección 0: Requisitos previos"
description: "Crea tu propia copia de Tailspin Toys y prepara un GitHub Codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [codespace][codespaces]. En la siguiente lección utilizarás su terminal integrado para instalar y ejecutar Copilot CLI.

En esta lección:

- Crearás una copia propia del proyecto Tailspin Toys a partir de la plantilla.
- Crearás un Codespace y confirmarás que el proyecto está listo.

## Configura el repositorio del taller

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora a partir de la [plantilla del repositorio][tailspin-template]. El nuevo repositorio contiene todos los archivos necesarios para el taller.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.
3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`), ya que la necesitarás más adelante en el taller.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se generará automáticamente un backlog de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

Utiliza una copia nueva de la plantilla del taller. Incluye las instrucciones del repositorio, el código de la aplicación, las pruebas, una habilidad `quality-checks` y el backlog que utilizarás. Si usas una copia anterior, comprueba con quien imparte el taller que contiene los archivos que necesitas.

## Crea un Codespace

A continuación, utilizarás un Codespace para completar el taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un editor completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración del Codespace. La plantilla instalará las dependencias del proyecto, Chromium de Playwright y la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y próximos pasos

Ya tienes el entorno preparado. En esta lección:

- Creaste una copia del proyecto Tailspin Toys a partir de la plantilla.
- Creaste un Codespace y confirmaste que el proyecto estaba listo.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en el Codespace y te autenticarás con tu cuenta de GitHub.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crea un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
