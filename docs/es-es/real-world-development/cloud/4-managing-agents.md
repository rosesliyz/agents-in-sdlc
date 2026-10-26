---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Tienes varias herramientas para administrar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes consultar las operaciones realizadas y orientar una sesión que siga en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Ya que se estaba actualizando el estilo del sitio y añadiendo la funcionalidad para cambiar de modo, parecía lógico incluir esta opción. Orientarás el trabajo del agente para que añada tanto el modo claro como el modo de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

La página de agentes muestra el estado actual de todas las tareas asignadas al agente en la nube de Copilot:

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación de la base de código;
    - generar interfaces de programación de aplicaciones (API) para modificar juegos;
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Entre ellas puede haber:
    - obtener el código del repositorio;
    - preparar el entorno de trabajo de Copilot;
    - configurar servidores del Protocolo de contexto de modelo (MCP);
    - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que se muestran dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios de la derecha. Desde ahí puedes ver la solicitud y los archivos modificados para supervisar el trabajo.

## Orientar al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya usado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta solicitud:

    ```plaintext
    Mientras trabajamos en un modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo con el que los usuarios puedan seleccionar el modo de visualización que prefieran.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la solicitud.
5. Observa cómo Copilot confirma la solicitud y la incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba realizando en el momento en que enviaste el mensaje.

Este proceso también tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde este centro de control puedes hacer un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o proporcionar indicaciones adicionales.

- Has explorado el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- Has redirigido una sesión en curso para solicitar una funcionalidad adicional.

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
