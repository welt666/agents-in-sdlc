---
title: "Ejercicio 4: Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las acciones realizadas e incluso guiar una sesión en curso.

En esta lección:

- Explorarás la página de agentes para supervisar las tareas del agente en la nube.
- Guiarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo se ha dado cuenta de que también sería un buen momento para añadir un modo claro. Como ya se está trabajando en los estilos del sitio y en la funcionalidad de alternancia, parece lógico incluir también esta opción. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Comprueba el estado de todas las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Examina las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código base.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - Obtener el código del repositorio.
   - Preparar el entorno de trabajo.
   - Configurar servidores MCP.
   - Realizar varias tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos concretos que se muestran dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios (PR), que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir supervisando el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras implementa el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que la sesión termine de configurarse y empiece a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que ha recibido el mensaje y lo incorpora a su flujo de trabajo.

## Deja trabajar a Copilot

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba ejecutando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y guiar las tareas del agente de GitHub Copilot en la nube. Puedes realizar el seguimiento de todas las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso.

Has explorado estos conceptos:

- Supervisar las tareas del agente en la nube desde mission control y la página de agentes.
- Redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
