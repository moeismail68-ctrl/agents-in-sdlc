---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en buscar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto, actualizarlo y volver a empezar.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentra. Veamos cómo utilizar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya has añadido un asistente para obtener las editoriales. Ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web para conocer las funciones disponibles.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Toda la aplicación está implementada con Astro, que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los asistentes de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante scripts Astro `<script>` pequeños y específicos que utilizan las API DOM estándar.

### Inicia el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal dentro de Codespaces con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá una indicación parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace a una URL local desde el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, donde se está ejecutando el servidor web.

### Explora el sitio web

Cuando se esté ejecutando, podrás explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre su página de detalles con el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

El sitio inicial funciona, pero queremos añadir capacidades. Empecemos por el backlog. Al crear el proyecto desde la plantilla se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que la muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no prefieren un modelo sobre otro. Durante su elaboración se utilizó Claude Sonnet 4.5, por lo que se incluye en estas instrucciones para obtener sugerencias relativamente coherentes y ofrecer una buena experiencia. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es posible que las sugerencias no coincidan con los ejemplos del taller. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot podría utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre el backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedirle al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad para probar el código y accesibilidad;
   - enlaces a archivos de instrucciones de tipos concretos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios. El modo de agente de Copilot puede encargarse de estas tareas.

1. Puedes continuar la conversación actual o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y busca los archivos pertinentes: asistentes de acceso a datos, interfaz de usuario y pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar varios minutos, según la complejidad de la tarea y del código base. Durante el trabajo pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás esas opciones para todos los cambios; no es necesario seleccionarlas mientras sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completada del modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has aprendido a:

- utilizar el modo de agente para implementar funcionalidades en la interfaz y la capa de datos;
- explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas antes de combinarlos en el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario implementar la paginación. Utiliza lo que has aprendido para pedir a Copilot que actualice el sitio. Al planificar el código:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo;
- proporciona a Copilot las directrices necesarias para crear la implementación que quieres;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; para qué sirven y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
