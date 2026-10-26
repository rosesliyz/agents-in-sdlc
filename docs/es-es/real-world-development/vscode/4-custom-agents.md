---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial (IA) especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio que orienten a Copilot para que realice determinadas tareas con más eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de accesibilidad para el contenido web (WCAG), uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con al menos los campos `name` y `description`, seguidos de una instrucción en Markdown que define el comportamiento, los conocimientos y las pautas del agente.

### Agentes personalizados y skills de agente

Los agentes personalizados y las [skills de agente][agent-skills-concept] tienen algunos aspectos en común. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La distinción más sencilla es que el **agente personalizado** es quien trabaja y las **skills** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar skills —e incluso otros agentes— mientras realizan el trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad. Para ello, podría recurrir a skills como un flujo de trabajo para solicitudes de incorporación de cambios o una skill que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier aspecto de la IA, prueba y ajusta el enfoque a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos comentarios recientes indican que a ciertas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función lo antes posible. Para generarla, usarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para tareas de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve al codespace para preparar una rama de trabajo y revisar el agente personalizado de accesibilidad:

1. Abre un terminal y crea una rama nueva basada en `main` para mantener separado el trabajo de accesibilidad de la solicitud de filtrado del ejercicio 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Fíjate en los metadatos YAML `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en los metadatos de los agentes personalizados.

4. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código del lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript;
   - errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes de la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para que las instrucciones y herramientas de ese agente se apliquen a la sesión de chat.

> [!TIP]
> **Abrir Copilot Chat**
>
> Una conversación nueva proporciona un contexto limpio. El modo y el modelo varían según el ejercicio; los pasos lo indican cuando es necesario.

1. Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat**.
2. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** si todavía no está seleccionado.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

3. Selecciona el menú desplegable de agentes de la parte inferior de la vista Chat. Muestra el agente activo; de forma predeterminada, es **default**.
4. Selecciona **Accessibility agent** en la lista de agentes disponibles.
5. Envía esta solicitud al agente de accesibilidad:

    ```plaintext
    Añade un modo de alto contraste al sitio. Debe haber un control para activarlo y desactivarlo, y la selección debe conservarse entre recargas de la página mediante el almacenamiento local del navegador.
    ```

6. Copilot Chat empezará a trabajar: explorará la base de código, propondrá cambios y aplicará modificaciones a los archivos del proyecto. Cada cambio aparecerá en el chat, junto con la ruta del archivo y un diff para que puedas revisarlo.
7. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** con los archivos que haya modificado el agente.

> [!NOTE]
> Es probable que el proceso tarde unos minutos. Copilot está haciendo cambios reales en el repositorio: puede editar archivos existentes, como componentes Astro, hojas de estilo y pruebas relacionadas.

Revisarás y orientarás este trabajo mientras esté en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Con ellos puedes convertir los conocimientos y estándares de tu equipo en agentes reutilizables que guíen a Copilot para realizar determinados tipos de trabajo con más eficacia.

- Has revisado cómo se definen los agentes personalizados.
- Has usado un agente personalizado en el modo Agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se apliquen y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de los agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y administrar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
