---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de recursos. El agente de GitHub Copilot en la nube puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así, puede encargarse de tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en asuntos de mayor alcance.

Con el agente de Copilot en la nube explorarás:

- Cómo personalizar el entorno para generar código.
- Cómo garantizar que las operaciones se realicen de forma segura.
- La importancia de definir incidencias con un alcance claro.
- Cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que prácticamente no existe. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar la exploración de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los usuarios puedan seguir explorando. No necesita una implementación pulida; solo algo que permita hacer pruebas de aceptación de la experiencia de usuario. Ahora mismo, la falta de esta funcionalidad es un impedimento, aunque hay otras incidencias de mayor prioridad.

Estas tareas pueden perder prioridad con facilidad, por lo que son buenas candidatas para asignarlas al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el resultado y comprobar que está todo como se esperaba.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Como ocurre al trabajar con una persona, puedes asignarle tareas de distintas formas, por ejemplo, [asignándole una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot creará una solicitud de incorporación de cambios en borrador para hacer el seguimiento, preparará el entorno y empezará a trabajar. Puedes consultar la sesión mientras está en curso o una vez terminada. Cuando la solución esté lista para que la revises, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las tareas que parecen sencillas pueden ser complejas al analizarlas en profundidad. Por eso, [define cuidadosamente el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: divide el trabajo en fases, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; en caso contrario, aprovecha para echarle un vistazo. Antes de asignar tareas al agente en la nube, revisa en modo de solo lectura los archivos de instrucciones que incluye el repositorio. Así podrás observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya tengas en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, busca en la PR resultante los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones. Volveremos a señalarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

La creación de código suele requerir un entorno específico y la ejecución de algunos scripts de configuración para que todo funcione correctamente, independientemente de quién realice el trabajo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. El flujo ya está configurado para que el taller avance y permita aprender. Proporciona a Copilot Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio durante la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio durante la configuración, Copilot lo hará automáticamente al terminar los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay algunos puntos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Esto sirve para probarlo sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta mantener el código o incorporar nuevos desarrolladores al equipo. Copilot destaca en la generación de documentación, así que esta es una buena incidencia para asignarle al agente de Copilot en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior revisarás el resultado.

1. Abre el repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se ha generado. Tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera un momento y vuelve a cargar la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno —ejecutando el flujo de trabajo anterior—, analizando el proyecto y determinando el mejor enfoque para la tarea.

13. Revisa la lista y las tareas que realizará Copilot.
14. Desplázate por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que volver a cargar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo resuelve Copilot el problema. Explora el código y comprende el estado del proyecto, se detiene para analizar y elegir un plan adecuado, y también crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente de Copilot en la nube es realizar tareas de forma asíncrona y permitirnos ocuparnos de otros asuntos. Aprovecharemos esa característica para asignarle otra tarea y, mientras trabaja, nos centraremos en escribir código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente de Copilot en la nube es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Puede que añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. Asignemos el trabajo al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, aparecerán varios ojos 👀 en el primer comentario de la incidencia. Indican que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software: no necesitas supervisarlo activamente, sino revisar el resultado cuando termine. Ahora puedes crear y utilizar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignar incidencias a Copilot para que las resuelva de forma asíncrona. El agente puede ayudar a reducir deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- Personalizar el entorno para generar código.
- Garantizar que las operaciones se realicen de forma segura.
- Definir incidencias con un alcance claro.
- Asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y recibe instrucciones personalizadas, que ya exploramos en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
