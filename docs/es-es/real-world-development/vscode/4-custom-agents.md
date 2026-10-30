---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de desarrollo. Al definir agentes en archivos Markdown dentro de `.github/agents`, puedes proporcionar instrucciones concretas, prácticas recomendadas, patrones de programación y conocimientos del dominio para orientar al agente. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que garantice el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos suelen definirse en archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará el sitio y lo actualizará de acuerdo con las directrices de accesibilidad; para hacerlo, podría utilizar habilidades como una que gestione solicitudes de incorporación de cambios o ejecute pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las más adecuadas para tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Los comentarios de algunos usuarios indican que el tema oscuro actual es difícil de leer debido al contraste insuficiente entre el texto y el fondo. Para solucionar el problema, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que quieres implementar esta funcionalidad cuanto antes. Para generarla, utilizarás un agente personalizado.

En este ejercicio:

- Revisarás un agente personalizado de accesibilidad existente.
- Usarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu codespace, abre un terminal y crea una rama nueva a partir de `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - Las responsabilidades principales al generar código para un sitio web accesible.
   - Las prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Errores habituales que se deben evitar.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, situado en la parte inferior de Copilot Chat. Puedes seleccionar un agente personalizado para aplicar sus instrucciones y herramientas a una sesión de chat.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los siguientes pasos, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el menú desplegable de agentes en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y pruebas relacionadas.

Revisarás y guiarás este trabajo en curso en el siguiente ejercicio.

## Resumen y próximos pasos

En este ejercicio has utilizado los [agentes personalizados][custom-agents] de GitHub Copilot. Estos asistentes de IA se especializan en tareas y áreas concretas. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- Cómo se definen los agentes personalizados.
- Cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se produzcan y añadirás un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crea agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisa y gestiona agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
