---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] te permiten proporcionar esa guía para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas;
- añadirás un estándar general para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no ejecutarás un prompt *antes y después*: el agente de GitHub Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar su efecto en tiempo real. Verás cómo influye el archivo de instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios que produzca el agente.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar el modo oscuro y tener un aspecto moderno.
- La documentación debe añadirse al código mediante comentarios TSDoc.
- Al principio de cada archivo debe incluirse un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar las sugerencias y los fragmentos de código para que sean más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de contexto que sea pertinente para la mayoría de las conversaciones o solicitudes de CLI enviadas a Copilot, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se crean para tareas o tipos de archivo concretos. Puedes usarlos para proporcionar directrices sobre lenguajes (como TypeScript o Astro) o tareas (como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias).

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot también [admite otros estándares de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Limita `copilot-instructions.md` a las directrices del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares generales de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de cómo debería (y no debería) ser el código.

No existe una única forma correcta de escribir archivos de instrucciones, igual que no existe una única manera de usar la IA. La experimentación te permitirá averiguar qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar este proyecto, quizá encuentres archivos para muchos tipos de tareas, como las [actualizaciones de interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma diferente (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente te indicará cuándo utilizarla.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, se aplica a los archivos de prueba TypeScript que coincidan, por ejemplo, con `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias para este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios en los archivos.

## Añade un nuevo estándar al repositorio

Ahora añadirás una regla general para todo el proyecto: la documentación debe incluirse en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube la aplicará a las incidencias que le asignes más adelante en este recorrido.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá tus instrucciones de la rama a la que se dirija la incidencia, pero así también confirmarás tus cambios de forma ordenada:

1. En el terminal de tu codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha explicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añadamos estándares de programación para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Verás que documenta los estándares de programación, pero aún no exige documentación dentro del código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes para indicar a Copilot que añada encabezados de comentarios a los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, Copilot creará una rama a partir de `main`. Por eso, los cambios de instrucciones deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá leer estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir el paso de la rama y confirmar los cambios directamente en `main`. El uso de una rama permite que el taller se parezca más al trabajo en un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto de este recorrido.

A continuación, [asigna tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

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
