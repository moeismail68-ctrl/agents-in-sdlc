---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas relacionadas con la mejora de la experiencia de usuario y la incorporación de funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este panel puedes ver todas las tareas de agente que tienen solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas e incluso guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- guiarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo una opción de alternancia, parecía lógico incluirlo. Guiarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Veamos el estado actual de las tareas que has asignado al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en ejecución. Revisa las operaciones realizadas por Copilot, que pueden incluir:
   - descargar el código del repositorio;
   - preparar el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Allí puedes ver la PR y los archivos modificados para seguir el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que utilice Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe el siguiente prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo añade a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot trabajará en la tarea actualizada. Incorporará la nueva solicitud a su flujo de trabajo cuando termine el paso que estaba ejecutando al recibir el mensaje.

El proceso también tardará varios minutos; puedes aprovechar la pausa para repasar lo que has aprendido y explorado.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente de GitHub Copilot en la nube. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo realizado y redirigir tareas en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- utilizar el panel de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
