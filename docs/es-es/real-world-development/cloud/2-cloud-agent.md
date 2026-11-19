---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- crear incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. El primer paso es añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados —otros títulos de la misma categoría— para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida; solo algo que permita hacer pruebas de aceptación de la experiencia. Este trabajo es un impedimento, pero hay otras incidencias de mayor prioridad.

Estas son tareas que suelen quedar relegadas y que se pueden asignar al agente de GitHub Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas. Después, podrás volver para revisar su trabajo y confirmar que cumple tus expectativas.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Igual que con una persona, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una PR en borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes inspeccionar la sesión mientras se ejecuta o después de que termine. Cuando esté lista la solución propuesta, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas aparentemente sencillas pueden ser complejas al examinarlas en detalle, así que [define cuidadosamente el alcance de las tareas que asignas al agente en la nube][cloud-agent-best-practices]. Trátalo como a un programador colaborativo: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido cómo guían a Copilot las instrucciones personalizadas. Si ya has realizado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones que ya incluye este repositorio, sin modificarlos, para observar después su efecto.

Abre los siguientes archivos en la interfaz web de GitHub de tu repositorio o en un codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las directrices sobre las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, comprueba si la PR resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones; volveremos a mencionarlas al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

Crear código, tanto si lo hace una persona como si lo hace un agente, suele requerir un entorno específico y algunos scripts de configuración. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así, puede acceder a las herramientas y dependencias necesarias. Este flujo de trabajo ya está configurado para facilitar el taller: proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos necesarios para los pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (toda la aplicación utiliza Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que se ejecuta correctamente sin esperar a que lo inicie Copilot.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen por completo de ella. Esta deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot destaca en la generación de documentación, así que esta es una buena incidencia para asignarle al agente en la nube. Trabajará en segundo plano y podrás revisar el resultado en un ejercicio futuro.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está configurando el entorno (ejecuta el flujo de trabajo descrito antes), analizando el proyecto y determinando cómo resolver la tarea.

13. Revisa la lista y las tareas que Copilot tiene previsto completar.
14. Desplázate hacia abajo por la cronología de la solicitud. Verás una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende el estado del proyecto, se detiene a pensar y decide un plan antes de crear código.

El proceso puede tardar varios minutos. Una ventaja principal del agente en la nube es que realiza tareas de forma asíncrona, de modo que puedes ocuparte de otros asuntos. Aprovecharemos esa función: asignaremos otra tarea al agente y, mientras trabaja, añadiremos funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente de GitHub Copilot en la nube es que permite repartir el trabajo para que tú puedas ocuparte de unas tareas mientras el agente se encarga de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, podrías aprovechar ese tiempo para otras tareas. Asignémosela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás varios emoji 👀 en el primer comentario de la incidencia. Indican que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software: no hace falta supervisarlo continuamente; puedes revisar el resultado cuando termine. Mientras tanto, vamos a crear y utilizar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. También puedes utilizarlo para abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- crear incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes empezar a crear y utilizar agentes personalizados. [El agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y recibe instrucciones personalizadas, que ya has explorado en otros módulos.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
