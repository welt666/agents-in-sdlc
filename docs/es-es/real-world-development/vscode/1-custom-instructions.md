---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ella. Hay varias formas de compartir contexto con Copilot; una de las más importantes son los [archivos de instrucciones][instruction-files], que proporcionan directrices para generar código.

En este ejercicio:

- Explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones asociados a rutas.
- Generarás el primer componente de datos para el filtrado (una función auxiliar para editores) con las instrucciones actuales.
- Añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.
- Volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- Confirmarás en `main` la actualización de instrucciones y el primer componente de filtrado para que Copilot utilice las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares establecidos. Copilot no es determinista. El objetivo es observar la tendencia del cambio de comportamiento tras actualizar las instrucciones, no obtener siempre el mismo resultado exacto.

## Archivos de instrucciones

### Escenario

Tailspin Toys cuenta con directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su función.

Los archivos de instrucciones permiten proporcionar a Copilot el contexto necesario para realizar el trabajo de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas aportan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos. Permiten orientar sus sugerencias y fragmentos de código. Puedes especificar convenciones de programación, bibliotecas e incluso el tipo de comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción de lo que se desarrolla, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` pueden proporcionar directrices para lenguajes, tipos de archivo o tareas concretos, como crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md, tal como se explica en la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

No es posible explicar aquí en profundidad cómo crear archivos de instrucciones. Los ejemplos del proyecto muestran un enfoque representativo. En general:

- Limita `copilot-instructions.md` a las directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para directrices específicas de tipos de archivo —pruebas unitarias, componentes de Astro o capa de datos— o de tareas.
- Usa lenguaje natural y presenta directrices claras con ejemplos de cómo debería y cómo no debería ser el código.

No hay una única forma correcta de crear archivos de instrucciones, igual que tampoco hay una sola forma de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería contar con una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que encuentres archivos para tareas como [actualizar la interfaz][ui-instructions] o trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta: por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando te indicará cuándo es pertinente.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para los flujos de trabajo de GitHub Actions. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón relativo a la raíz del repositorio que determina qué archivos coinciden. Por ejemplo, se aplica a los archivos de prueba TypeScript que coinciden con `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir las instrucciones extensas en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás directrices para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los siguientes pasos, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, a menudo tanto en el archivo auxiliar como en las pruebas.
6. Observa que la función auxiliar propuesta es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentarios del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añade un estándar al repositorio

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade un estándar de programación para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no incluye directrices sobre la documentación en el código.
3. Añade estas líneas Markdown justo después de los estándares existentes para pedir a Copilot que cree encabezados de comentarios y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden guardarse en `.github/copilot-instructions.md` para proporcionar directrices generales, o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo general del repositorio es el lugar adecuado para estándares comunes, como la regla sobre los comentarios de documentación.

## Vuelve a ejecutar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Haz clic de nuevo en `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones se gestionan con el mismo control de versiones que otros archivos del repositorio. Confirma los cambios y envía la rama.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, comprueba que los cambios en las instrucciones y en la función auxiliar están presentes:

   ```bash
   git status
   ```

3. Añade al área de preparación y confirma la actualización de las instrucciones y todos los archivos que Copilot haya modificado para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Una vez enviada la rama, crea una solicitud de incorporación de cambios asociada a la incidencia del backlog sobre los estándares de documentación. Puedes hacerlo manualmente, pero Copilot también puede localizar la incidencia, crear la PR para resolverla y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub al área de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las instrucciones para conceder acceso. En un ejercicio posterior aprenderás cómo funciona esta conexión con Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR desde la rama actual. Indícale que describa tanto las actualizaciones como la nueva función auxiliar de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Si Copilot te pide **Allow** para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si procede.
6. Cuando se cree la PR, pídele que la combine y vuelva a cambiar la rama local a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide **Allow** para realizar acciones en GitHub o ejecutar comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y, después, has usado Copilot Chat en VS Code para:

- Enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- Añadir un estándar general al repositorio en `.github/copilot-instructions.md`.
- Volver a enviar el mismo prompt y comprobar cómo el código propuesto sigue el nuevo estándar.
- Confirmar en `main` la actualización de instrucciones y la función auxiliar inicial para que el siguiente ejercicio pueda utilizarla.

A continuación, aplicarás las instrucciones en [el modo de agente][next-lesson] mientras Copilot añade una funcionalidad en varios archivos del proyecto.

## Recursos

- [Instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
