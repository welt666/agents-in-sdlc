---
title: "Ejercicio 2: Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos en una aplicación suelen requerir modificar varios archivos y realizar tareas como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. Y así sucesivamente.

Aquí es donde entra en juego el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias diferentes. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- Dejar que el modo de agente de Copilot explore el proyecto, identifique los archivos pertinentes y coordine los cambios.
- Implementar funcionalidades en la interfaz y en la capa de datos con el modo de agente.
- Revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos en el código base.

## Escenario

A medida que aumenta la lista de juegos, quieres que los usuarios puedan filtrarla por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para obtener los editores; ahora completarás el resto del trabajo de datos, interfaz y pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que utilizan las API estándar del DOM.

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al servidor web que se ejecuta en el codespace.

### Explora el sitio web

Con el sitio en marcha, explora sus funciones. Entre otras, incluye:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accedes a una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los siguientes pasos, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

La implementación inicial del sitio funciona, pero queremos añadir más funcionalidades. Empecemos por el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Durante su desarrollo utilizamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones para que las sugerencias sean relativamente coherentes y la experiencia sea más uniforme. Como los modelos de lenguaje son probabilísticos, las sugerencias pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot puede utilizar un comando MCP diferente y aun así completar la tarea.

3. Envía este prompt a Copilot para preguntarle por el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que proporcionará el contexto necesario. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Lee las directrices para añadir funcionalidad. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, pruebas y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar directrices, dividir conjuntos complejos en partes más manejables y señalar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer, como mínimo, tres cambios en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas para el comportamiento del filtrado.
- Actualizar la página de listado de juegos con la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo de agente de Copilot puede realizar este trabajo.

1. Continúa en la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de Chat.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar tanto los archivos de la capa de datos y la interfaz como las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos, según la complejidad de la tarea y del código base. Mientras tanto, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, tendrás una opción **Keep** o **Undo** para cada cambio; no tienes que seleccionarlas mientras el trabajo siga en curso.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir funcionalidad al sitio web Tailspin Toys. Has visto cómo:

- El modo de agente de GitHub Copilot implementa nuevas funcionalidades en la interfaz y la capa de datos.
- El modo de agente explora el proyecto, identifica los archivos pertinentes y coordina los cambios.
- Se revisan los cambios y las pruebas generados por el modo de agente antes de combinarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: Implementa la paginación

A medida que aumenta la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que implemente la paginación. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- Decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- Al crear el prompt, proporciona a Copilot las directrices necesarias para implementar el comportamiento que quieres.
- Puede que tengas que iterar con GitHub Copilot, pedir cambios y aportar contexto. Es el proceso normal cuando se trabaja con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
