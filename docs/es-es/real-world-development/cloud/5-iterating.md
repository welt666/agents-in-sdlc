---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Revisemos los cambios y, si hace falta, demos indicaciones a Copilot para mejorarlos.

### Escenario

Como ya hemos señalado, la IA generativa no cambia los fundamentos del diseño de software y DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Por tanto, antes de habilitar la revisión para el resto del equipo, revisaremos las sugerencias de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad para que el trabajo se realice de forma segura:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que ha creado el agente de GitHub Copilot en la nube: la documentación del código. Para revisarla, utilizarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que veas una advertencia que indica que el firewall ha bloqueado a GitHub Copilot. Es normal: el acceso del agente en la nube a recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de Copilot en la nube][agent-firewall].

1. Abre el repositorio en una nueva pestaña del navegador.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos. Mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que ahora incluye comentarios de documentación TSDoc y otras mejoras. Los cambios exactos pueden variar.

    Mientras revisas las diferencias, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que informa de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, igual que mencionarías a otro miembro del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no podemos indicar un texto concreto. Algunos ejemplos de cambios que podrías pedir:

- Añadir a la parte superior de cada archivo de código encabezados de comentarios con una breve explicación de su función.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot** como lo harías con cualquier otro usuario. Usa una de las sugerencias anteriores o pide otro cambio de documentación que quieras ver en el código base.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Botón Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, convierte la PR de borrador en lista para revisión y combínala en la rama principal.

![Convierte la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot ha creado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que informa de que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** Puedes cambiar a esta rama en Codespaces para probar manualmente la funcionalidad de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente en la nube y solicitar cambios, por ejemplo, modificar el número de juegos relacionados o añadir encabezados de comentarios y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado desde mission control en el ejercicio 4.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - Los controles para cambiar de modo.
   - El uso del almacenamiento local para guardar las preferencias.
   - Los cambios de CSS o estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - El código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que informa de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** Puedes cambiar a esta rama en Codespaces para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta los comandos siguientes; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se mantienen al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas. Si detectas problemas de accesibilidad o posibles mejoras, menciona a **@copilot** en un comentario y solicita los cambios, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle tareas reales del backlog y ocuparte de otra cosa. Para practicar, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para mostrar interés en un juego en la página de detalles.
- Implementar la paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados del repositorio** para observar más adelante su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** de accesibilidad para añadir controles de los modos de alto contraste y claro.
- **Utilizaste la página de agentes de Copilot como mission control** para supervisar y guiar la sesión de accesibilidad en curso.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionaste a `@copilot` para solicitar cambios y aprobaste flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configura el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
