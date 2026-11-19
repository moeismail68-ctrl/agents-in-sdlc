---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como realiza ese trabajo en tu equipo y en tiempo real, Copilot Chat de VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y guiar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones de archivos propuestas;
- guiarás al agente durante la sesión para que añada una opción de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, pensado para quienes consideran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en línea en el chat, con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas. Aparecerán lecturas y ediciones de archivos, además de los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al finalizar, debería mostrar un mensaje con un resumen de los cambios.

## Guía al agente para añadir el modo claro

Ahora que el modo de alto contraste está listo, ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. En la entrada de Copilot Chat, envía el siguiente prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios existentes y propondrá actualizaciones para el modo claro.
3. Revisa cada nueva edición y utiliza **Keep** o **Undo** en cada diferencia, como hiciste con el modo de alto contraste.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta rediseñar los controles de alternancia— puedes detenerlo con el botón **Stop** junto a la entrada de chat y enviar un mensaje aclaratorio.

> [!TIP]
> Guiar una sesión activa es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para pedir ajustes («simplifica el CSS: utiliza variables CSS en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones de componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se han aplicado las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y almacenamiento local para guardar la selección.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición al aplicarse, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones de archivos individuales;
- has guiado una sesión en curso con prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que haya generado el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
