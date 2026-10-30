---
title: "Lección 4: Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe hacerse de una forma concreta, asegúrate de que Copilot pueda acceder a las instrucciones correspondientes. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha seguido Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- Explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas para determinadas rutas.
- Actualizarás el archivo de instrucciones para que se respeten los estándares de programación.
- Verás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys cuenta con directrices y requisitos para las prácticas de desarrollo, como los siguientes:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar el contrato de `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Los archivos de instrucciones permiten proporcionar a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas aportan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos. Permiten orientar las sugerencias y los fragmentos de código. Puedes especificar convenciones de programación, bibliotecas e incluso el tipo de comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizados del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para comprobar el efecto que tienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la descripción breve del proyecto y sus directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y a archivos del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir.

## Actualiza los archivos de instrucciones para reflejar las directrices del equipo

Los archivos existentes son un buen punto de partida, pero aún falta una directriz. Modifica el archivo principal `copilot-instructions.md` para exigir comentarios TSDoc en el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade esta viñeta al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y vuelve a abrirla para que Copilot cargue las directrices nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está abierta la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código con precisión.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI utiliza las instrucciones del repositorio y has aplicado un nuevo estándar a tu funcionalidad. En concreto:

- Exploraste cómo llegan al agente las instrucciones del repositorio y las específicas para determinadas rutas.
- Actualizaste el archivo de instrucciones para que se respeten los estándares de programación.
- Comprobaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añade instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
