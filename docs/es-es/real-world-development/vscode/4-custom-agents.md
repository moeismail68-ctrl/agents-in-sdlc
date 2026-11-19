---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o áreas concretas del flujo de desarrollo. Puedes definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio y proporcionar a Copilot instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para realizar ciertos tipos de trabajo con mayor eficacia. Los equipos pueden plasmar sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique las directrices de [WCAG][wcag], otro de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown en `.github/agents` dentro del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, como mínimo, `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más clara es que un **agente personalizado** es el trabajador y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y se encargan de coordinar las habilidades e incluso otros agentes para realizar el trabajo. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Durante la tarea podría llamar a habilidades como una para gestionar solicitudes de incorporación de cambios o para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única manera correcta de escribir un agente personalizado. Como ocurre con cualquier función de IA, prueba distintas opciones e itera para encontrar lo que funciona en tu entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que utilizarás un agente personalizado para generar esta funcionalidad lo antes posible.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revísalo para entender cómo guiará a Copilot.

Vuelve al codespace, abre un terminal y crea una rama nueva a partir de `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

1. En el menú desplegable de agentes de Chat, selecciona **Agent** si aún no está seleccionado.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes de la parte inferior de la vista Chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía el siguiente prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat empezará a trabajar: explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. A medida que se apliquen los cambios, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot realizará cambios reales en el repositorio y editará componentes Astro, CSS y las pruebas relacionadas.

En el ejercicio siguiente revisarás y guiarás el trabajo en curso.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y áreas concretas. Con ellos puedes codificar los conocimientos y estándares del equipo en agentes reutilizables que guían a Copilot para realizar ciertos tipos de trabajo con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se aplican y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
