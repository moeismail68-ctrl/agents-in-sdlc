---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas;
- generarás la primera parte de la capa de datos para el filtrado (un asistente de editoriales) con las instrucciones actuales;
- añadirás un estándar general para todo el repositorio en `.github/copilot-instructions.md`;
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar;
- confirmarás la actualización de instrucciones y la parte de filtrado en `main` para que Copilot pueda utilizar las directrices actualizadas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del cambio de comportamiento tras actualizar las instrucciones, no obtener siempre el mismo resultado exacto.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar el modo oscuro y tener un aspecto moderno.
- La documentación debe añadirse al código mediante comentarios TSDoc.
- Al principio de cada archivo debe incluirse un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar las sugerencias y los fragmentos de código para que sean más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de contexto pertinente para la mayoría de las conversaciones o solicitudes, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se crean para tareas o tipos de archivo concretos. Puedes utilizarlos para proporcionar directrices sobre lenguajes (como TypeScript o Astro) o tareas (como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias).

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
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes en archivos más pequeños y reutilizables y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios en los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el asistente.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía el siguiente prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, a menudo tanto al archivo auxiliar como a sus pruebas.
6. Observa que la función propuesta tiene tipos, acepta un cliente `db` como primer argumento y devuelve una matriz de editoriales con tipos. Estas características proceden de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un nuevo estándar al repositorio

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

## Vuelve a ejecutar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar la conversación.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
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

Has indicado a Copilot que siga un estándar nuevo y lo aplique a código real en el que se basará el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan mediante el mismo control de versiones que los demás archivos. Confirmemos y publiquemos la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que los cambios en las instrucciones y el asistente estén presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para la base del asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones adicionales (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ahora que has publicado la rama, crea una solicitud de incorporación de cambios y asóciala a la incidencia del backlog sobre las instrucciones. Copilot puede buscar la incidencia, crear la PR para cerrarla y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub a tu espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una herramienta de GitHub, VS Code podría pedirte que inicies sesión. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de archivos de instrucciones y que cree una PR desde la rama actual. Indícale que describa tanto las actualizaciones de instrucciones como la base del asistente de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida **Allow** para realizar acciones de GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
6. Cuando se cree la PR, pídele a Copilot que la combine y vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida **Allow** para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** cuando corresponda.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y después has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar general a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto sigue el nuevo estándar;
- confirmar la actualización de instrucciones y la base del asistente en `main` para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
