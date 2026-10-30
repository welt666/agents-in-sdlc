---
title: "Ejercicio 3: Prueba la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de recorrer la aplicación manualmente, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probarla. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- Aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- Añadirás el servidor MCP de Playwright al área de trabajo.
- Pedirás a Copilot que lo utilice para probar manualmente la funcionalidad de filtrado en un navegador.
- Publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a herramientas fuera del editor. Model Context Protocol (MCP) permite que Copilot lo haga: es un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y de su interacción con el contexto, el LLM y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Gracias a MCP, pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente y las herramientas o servicios externos, como API existentes o herramientas locales en paquetes de NPM. Cada servidor ofrece un conjunto diferente de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright. Permite navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a diferentes herramientas y recursos. GitHub ofrece un [registro MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Ya debería estar configurado el servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto ya incluye la entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no tienes que instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás otro servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo tenga este aspecto:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en el equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para sus pruebas de extremo a extremo, así que el navegador suele estar instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot solicita herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá que lo autorices cuando Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de recorrer la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros, leer los resultados y resumirlos. Es una forma rápida de comprobar que la funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados —botones, enlaces y elementos de lista— como lo haría una tecnología de asistencia. Así, una comprobación funcional sencilla también permite revisar la accesibilidad de forma básica.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te explicará lo que observa. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona como esperas, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir la PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Una vez que hayas confirmado que la funcionalidad funciona, abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarla. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Usa el botón **Sparkle** para generar un mensaje de commit.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una solicitud de incorporación de cambios desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, deja que la cree por ti. Copilot puede localizar la incidencia correspondiente y vincularla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. En el menú desplegable de agentes, comprueba que **Agent** está seleccionado para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para que Copilot recopile información y realice las operaciones. La primera vez que utilice una herramienta de GitHub, puede que tengas que autenticarte. Sigue las instrucciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace de Copilot para revisar la solicitud de incorporación de cambios, pero **no la combines todavía**.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio:

- Aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- Añadiste el servidor MCP de Playwright al área de trabajo.
- Lo utilizaste para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- Publicaste la rama y abriste una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado MCP para probar una funcionalidad, pero es solo una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
