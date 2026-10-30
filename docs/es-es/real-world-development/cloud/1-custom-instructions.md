---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ella. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices y ayudan a Copilot a entender no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- Explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones asociados a rutas.
- Añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no ejecutarás un prompt antes y después del cambio. El agente de GitHub Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influyen las instrucciones en las solicitudes de incorporación de cambios que genere más adelante.

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

## Explora los archivos de instrucciones personalizados del proyecto

El proyecto incluye un archivo `copilot-instructions.md` principal y varios archivos `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para los flujos de trabajo de GitHub Actions. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón relativo a la raíz del repositorio que determina qué archivos coinciden. Por ejemplo, se aplica a los archivos de prueba TypeScript que coinciden con `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir las instrucciones extensas en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás directrices para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Añade un estándar al repositorio

Ahora añadirás una directriz general para que Copilot incluya comentarios TSDoc y un encabezado de comentarios en el código nuevo.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero aún no incluye directrices sobre la documentación en el código.
3. Añade estas líneas Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden guardarse en `.github/copilot-instructions.md` para proporcionar directrices generales, o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo general del repositorio es el lugar adecuado para estándares comunes, como la regla sobre los comentarios de documentación.

## Confirma, envía y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot y el agente creará una rama desde `main`. Por tanto, las instrucciones deben estar en `main` para que el agente pueda utilizarlas.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá utilizar estas instrucciones en la siguiente lección.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama permite practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
