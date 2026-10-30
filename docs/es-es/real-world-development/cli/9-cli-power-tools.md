---
title: "Lección 9: Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de línea de comandos, GitHub Copilot CLI incluye numerosos comandos de barra. Estos permiten acceder a funciones avanzadas, consultar información interna y cambiar opciones de configuración. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección:

- Usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Usarás `/model` para explorar los modelos disponibles.
- Aprenderás cómo exportar o compartir una sesión con `/share`.
- Explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de la CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, opcionalmente, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puede que alcances el límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando lo necesita, y puedes inspeccionar o compactar el contexto manualmente con los comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es breve, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea no relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Los distintos modelos tienen capacidades diferentes y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar y seleccionar los modelos disponibles.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y consulta las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir una tarea en subtareas independientes, asignarlas a subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabajará de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Empieza por una tarea nueva y bien definida, y revisa el resultado con tu flujo de trabajo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar información interna. En esta lección:

- Usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto.
- Usaste `/model` para explorar los modelos disponibles.
- Aprendiste cómo `/share` puede exportar o compartir sesiones.
- Exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI ofrece más comandos de barra y funciones por descubrir. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y verás cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
