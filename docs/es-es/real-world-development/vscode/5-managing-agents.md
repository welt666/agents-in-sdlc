---
title: "Ejercicio 5: Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot trabaja en modo de agente, explora el código base, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- Supervisarás en tiempo real el trabajo del agente de accesibilidad.
- Revisarás y aceptarás las ediciones propuestas en los archivos.
- Guiarás al agente durante la sesión para añadir un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un modo claro para los usuarios que encuentran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, ampliarás el trabajo desde la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que el agente solicite ejecutar.
3. Cuando el agente proponga una edición, verás la diferencia en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, en la parte superior del chat, para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final con un resumen de los cambios.

## Guía al agente para añadir un modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. En el cuadro de entrada de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios existentes y propondrá nuevas modificaciones para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que hiciste con el modo de alto contraste. Selecciona **Keep** o **Undo** para cada diferencia.
4. Si el agente toma una dirección que no quieres —por ejemplo, si intenta reorganizar la interfaz de los botones de un modo que no te convence—, puedes detenerlo con el botón **Stop** junto al cuadro de entrada y enviarle un mensaje para aclarar lo que necesitas.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para solicitar ajustes («simplifica el CSS y usa variables en vez de duplicar selectores»), corregir el rumbo («no añadas un tercer botón; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos modos»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberían aparecer cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que el agente personalizado ha aplicado las prácticas de accesibilidad: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: revisaste las ediciones a medida que aparecían, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- La supervisión del trabajo del agente en tiempo real.
- La aceptación y el rechazo de ediciones individuales.
- La orientación de una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios que el agente personalizado local ha realizado durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
