---
title: "Lección 10: Repaso y próximos pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo en Tailspin Toys. En concreto:

- Preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado del backlog.
- Añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- Partiste de la incidencia de filtrado, definiste el enfoque en el modo Plan, implementaste la funcionalidad en el modo Autopilot y la revisaste en el modo Interactive.
- Guíaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` existente y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- Añadiste el servidor Model Context Protocol (MCP) de Playwright y lo utilizaste para explorar el filtrado en un navegador real.
- Creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- Revisaste el cambio completo de filtrado y autorizaste Agent Merge para la PR correspondiente.
- Exploraste comandos de barra para gestionar el contexto, los modelos y el uso compartido, y para delegar trabajo opcionalmente en la nube.

## Qué has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito cuando no hay valoración en las tarjetas de juegos.
2. **Flujo de filtrado y calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has usado la misma conversación y rama. Para agilizar el taller, hemos agrupado todo ese trabajo en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y pidiendo a Copilot que lo explorara con MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y de la cobertura antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir pruebas, también puede ser el resultado correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller, actualizaste las instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen perfiles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para decidir cómo abordar el trabajo antes de empezar, **Interactive** para seguir de cerca los cambios concretos y **Autopilot** para las tareas bien definidas. Elige un modelo más rápido para las modificaciones rutinarias y otro más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Explicar claramente *qué* hay que crear, *por qué* y *cómo* influye de forma notable en el resultado.

## Más cosas por explorar

Has completado el flujo principal. Aquí tienes algunas funciones más de la CLI:

- `/review` pide al agente de revisión de código que analice los cambios.
- `/rubber-duck` te permite explicar un problema y obtener otra perspectiva.
- `/fleet` coordina subtareas independientes en paralelo.
- `/worktree` aísla una tarea en otro árbol de trabajo.
- `/delegate` envía una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con una herramienta es seguir utilizándola: en código de producción, en proyectos personales o en esa pequeña aplicación que tienes en mente desde hace años. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Para ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personaliza Copilot CLI][customize-cli]
- [Gestiona solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
