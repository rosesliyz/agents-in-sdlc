---
title: "Ejercicio 5 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando activas el modo Agente de GitHub Copilot, trabaja de forma autónoma: explora la base de código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar cada diff en el editor, aceptar o rechazar cambios individuales y orientar la conversación con solicitudes posteriores para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- orientarás al agente durante la sesión para que añada un control para el modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado otra función: un control para el modo claro, para quienes prefieren una opción menos oscura que el tema predeterminado. En vez de empezar una conversación nueva, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat junto con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos, así como los comandos del terminal que solicite ejecutar.
3. Cuando el agente proponga editar un archivo, el diff aparecerá en el chat:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertir el cambio y permitir que el agente lo intente otra vez.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos que el agente ha modificado durante la sesión. Si una llamada a una herramienta solicita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

> [!NOTE]
> Una llamada a una herramienta puede solicitar permiso antes de ejecutarse.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Orientar al agente para que añada el modo claro

Ahora que está implementado el modo de alto contraste, amplía la misma conversación para añadir un control para el modo claro. Al continuar en la misma sesión de chat, el agente conserva todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón que configuró para el almacenamiento local.

1. En el cuadro de entrada de Copilot Chat, envía esta solicitud:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control para el modo claro junto al control de alto contraste. El modo claro debe seguir el mismo patrón: la selección debe guardarse en el almacenamiento local y el control debe estar en la misma zona de la interfaz. Los usuarios deben poder activar el alto contraste y el modo claro de forma independiente.
    ```

2. El agente retomará el trabajo, revisará los cambios realizados y propondrá cómo añadir el modo claro.
3. Revisa cada nuevo cambio a medida que aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diff.
4. Si el agente toma una dirección que no te convence —por ejemplo, intenta reorganizar los controles de una forma que no te gusta—, puedes detenerlo con el botón **Stop** junto al cuadro de entrada y enviarle una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de cero. Usa solicitudes posteriores para pedir ajustes («simplifica las hojas de estilo en cascada (CSS): usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa brevemente todo lo que haya modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos de los archivos modificados y revisa los diffs. Comprueba que se apliquen los patrones de accesibilidad del agente personalizado: atributos de Accessible Rich Internet Applications (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo Agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones a medida que aparecían, has aceptado o rechazado cambios y has enviado solicitudes posteriores para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones individuales;
- has orientado una sesión en curso mediante solicitudes posteriores.

En la [siguiente lección][next-lesson] publicarás tu trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás las modificaciones que ha generado el agente personalizado local durante este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo Agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
