---
title: "Lección 2 - Añade valoraciones: una mejora rápida"
description: "Haz un cambio pequeño en las tarjetas de juegos con Copilot CLI, revísalo en un navegador con un puerto reenviado y combina tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración con estrellas en los datos, pero aún no aparece en las tarjetas de la página de inicio. Pide al agente que la muestre, revisa el cambio y combínalo en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot específica en una rama de funcionalidad;
- pedirás al agente que haga un cambio pequeño en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación y comprobarás el cambio en un navegador con un puerto reenviado;
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como primer ejercicio, pedirás al agente que muestre en cada tarjeta la valoración existente. Es un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git seleccionados en el terminal, en vez de crear un árbol de trabajo dedicado. Puedes volver a las conversaciones guardadas, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás los prompts y las respuestas del agente, su actividad mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Iniciemos una conversación nueva para implementar la funcionalidad.

1. Vuelve al codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está abierto, inicia el programa con este comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía el siguiente prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, buscará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa las diferencias

Revisa todos los cambios generados por IA antes de combinarlos, aunque sean pequeños. Inspeccionémoslos en Copilot CLI.

1. Escribe `/diff` e inspecciona cada archivo modificado.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` tiene el valor `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que solucione los fallos.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Como Copilot, igual que otras herramientas de IA generativa, no es determinista, el código exacto puede variar. Revisa el comportamiento sin esperar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para examinar en el navegador la interfaz de usuario (UI) actualizada mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abre una solicitud de incorporación de cambios (PR) para añadir el código al proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI publicará la confirmación existente al crear la PR y mostrará la URL.
3. Para abrir la PR, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve al codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y próximos pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación de Copilot en una rama de funcionalidad;
- has pedido al agente que modifique las tarjetas de juegos;
- has revisado los cambios con `/diff`;
- has ejecutado la aplicación y comprobado las valoraciones en un navegador con un puerto reenviado;
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
