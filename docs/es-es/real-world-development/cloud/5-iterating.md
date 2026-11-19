---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades: documentar el código, crear una función de juegos relacionados para que el equipo de diseño pueda iterar e implementar mejoras de accesibilidad con modos de alto contraste y claro. Revisemos los cambios y, si hace falta, proporcionemos comentarios a Copilot para mejorarlos.

### Escenario

Como ya hemos explicado, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de habilitar la revisión para el resto del equipo, revisaremos las sugerencias de GitHub Copilot para crear documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente en la nube realiza sus tareas de forma asíncrona y sin supervisión, existen restricciones de seguridad para que el trabajo se mantenga protegido:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) que generó el agente de GitHub Copilot en la nube: añadir documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que aparezca un aviso de que el firewall bloquea a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine, lo que puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios exactos pueden variar.

    Mientras revisas, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones personalizadas que has revisado al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

El trabajo con Copilot no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, igual que harías con otros miembros del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay un texto de ejemplo que sirva para todos los casos. Puedes pedirle, por ejemplo:

- que añada al principio de cada archivo de código un encabezado con una breve descripción de su propósito;
- que añada comentarios TSDoc a archivos TypeScript y Astro;
- que cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario solicitando un cambio en la documentación generada y menciona a **@copilot**, igual que mencionarías a cualquier usuario. Utiliza una de las ideas anteriores u otra sugerencia relacionada con la documentación.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y combinarla en la rama principal.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot generó para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el indicador de flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse; puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente la funcionalidad de juegos relacionados. Abre el terminal del codespace y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes pedirle cambios mediante un comentario nuevo en la pestaña **Conversation**. Por ejemplo, puedes modificar el número de juegos que se muestran o pedir encabezados y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas).

## Revisa las mejoras de accesibilidad

Por último, revisemos las mejoras de accesibilidad que implementó el agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el panel de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de alternancia de los modos;
   - el uso del almacenamiento local para guardar las preferencias;
   - los cambios de CSS o de estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.);
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse; puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente las mejoras de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que la selección se mantiene al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones según las prácticas recomendadas. Si detectas problemas de accesibilidad o aspectos que se puedan mejorar, puedes mencionar a **@copilot** en un comentario y pedirle que los actualice, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar elementos reales del backlog mientras te ocupas de otras tareas. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para indicar interés en un juego en la página de detalles;
- implementar la paginación en la página de la lista de juegos;
- añadir validación de entradas y gestión de errores a los métodos de acceso a los datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para observar después su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con opciones de alto contraste y modo claro.
- **Has utilizado la página de agentes de Copilot como panel de control** para supervisar y guiar la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y la opción `/delegate` para volver a delegar en el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asignar al agente una incidencia de refactorización o cobertura de pruebas;
- crear un agente personalizado para otro ámbito;
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
