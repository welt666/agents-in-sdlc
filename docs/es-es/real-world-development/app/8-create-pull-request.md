---
title: "Lección 8: Crea y combina la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge y confirma que se combina la PR."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoraciones con estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- Inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la solicitud y resuelve lo que la bloquea: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y actualiza la base cuando hace falta. Después, la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando tras reiniciar la aplicación y se desactiva cuando la PR se combina.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar la PR sigue requiriendo tu autorización explícita. Revisa sus acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Usa Agent Merge para gestionar la PR

Una vez creado y revisado el código, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. El agente:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción.
- Actualizará la base o resolverá posibles conflictos de combinación con la rama de destino.
- Supervisará la integración continua para comprobar que se superan todas las comprobaciones.
- Supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizará los cambios necesarios para resolverlos.
- Puede combinar automáticamente la PR una vez completadas todas las tareas.

Dejaremos que Agent Merge combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el progreso y resuelve cualquier bloqueo que se indique. Antes de continuar, confirma que el estado de la PR es **Merged**; tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora también el proceso de la solicitud de incorporación de cambios. En esta lección:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- Inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizaste Agent Merge solo después de revisar los cambios y confirmaste que la PR se había combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar otra forma de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
