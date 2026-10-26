---
title: "Ejercicio 2 - Añadir funcionalidad con el modo Agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Normalmente, como persona desarrolladora, tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a actualizarlo. Y así sucesivamente.

Para eso sirve el modo Agente de Copilot.

El modo Agente está diseñado para trabajar con más autonomía en el entorno de desarrollo integrado (IDE). Se comporta de forma parecida a una persona desarrolladora: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentre. Veamos cómo usar el modo Agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo Agente y el agente en la nube están diseñados para experiencias distintas. El modo Agente trabaja en el IDE y permite recibir comentarios e interactuar rápidamente. El agente en la nube funciona como un compañero de programación asíncrono: trabaja como parte del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- Copilot en modo Agente puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios entre ellos;
- GitHub Copilot en modo Agente puede implementar funcionalidades nuevas en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por Copilot antes de integrar el trabajo en la base de código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que se filtren por categoría y editorial. Ya has añadido una función auxiliar para editoriales en el ejercicio anterior. Ahora completarás con Copilot en modo Agente el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo de software. Permite consultar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera las páginas como lenguaje de marcado de hipertexto (HTML) estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde sus metadatos frontmatter mediante Drizzle ORM. No hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz de usuario del lado del cliente. Los métodos reutilizables de acceso a datos están en `src/lib/`, y la interactividad se añade mediante pequeños elementos `<script>` específicos de Astro que usan las interfaces de programación de aplicaciones (API) estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

Para facilitar la ejecución del sitio web, se incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Sigue estos pasos para ejecutarla en GitHub Codespaces:

1. Vuelve al codespace. Al terminar el ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un aviso con una URL parecida a la siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

   Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace a la URL localhost desde el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio web en marcha, explora las funciones disponibles. Entre ellas:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles de un juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de tareas pendientes con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Una conversación nueva proporciona un contexto limpio. El modo y el modelo varían según el ejercicio; los pasos lo indican cuando es necesario.

La implementación inicial del sitio web funciona, pero queremos añadir nuevas funciones. La plantilla creó automáticamente una lista de incidencias para el proyecto; la siguiente secuencia te permitirá pedir a GitHub Copilot que te las muestre.

1. Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat**.
2. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

3. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Las personas autoras de este taller no prefieren un modelo frente a otro. Durante su desarrollo usamos Claude Sonnet 4.5 y lo indicamos aquí con la esperanza de obtener sugerencias de código relativamente coherentes y ofrecer una buena experiencia. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, puede que las sugerencias que recibas sean distintas de las que se describen en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes (LLM), Copilot podría usar un comando del Protocolo de contexto de modelo (MCP) distinto y aun así completar la tarea.

4. Envía a Copilot la siguiente solicitud para que muestre las incidencias pendientes:

   ```plaintext
   Muéstrame las tareas pendientes de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

5. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
6. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que proporcionará contexto para su trabajo. Usarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidades al sitio web.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales para añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura;
   - principios para diseñar componentes, facilitar las pruebas y garantizar la accesibilidad;
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, incluidos:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar pautas, dividir instrucciones complejas en partes pequeñas y manejables, y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, tendrás que realizar al menos tres actualizaciones distintas en la aplicación:

- añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de la lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y completarse correctamente antes de integrar los cambios en la base de código. El modo Agente de Copilot puede encargarse de estas tareas. Sigue estos pasos para implementar la funcionalidad:

1. Continúa en la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista de tareas pendientes. Usa esta solicitud:

   ```plaintext
   Actualiza el sitio para añadir filtros por editorial y categoría, según los requisitos de la incidencia relacionada de la lista de tareas pendientes de GitHub. Ya existe una función auxiliar para editoriales del ejercicio anterior, así que conserva y mejora ese trabajo si es necesario mientras completas lo que falta en la capa de datos, la interfaz de usuario y las pruebas. Antes de terminar, comprueba que todas las pruebas se ejecuten correctamente. El servidor ya está en marcha; no hace falta que lo inicies.
   ```

6. Observa cómo Copilot empieza explorando el proyecto y localizando los archivos relacionados con la funcionalidad. Debería encontrar tanto los métodos de la capa de datos y la interfaz de usuario como las pruebas. Después, empezará a modificar archivos y ejecutar pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y de la base de código, puede tardar unos minutos. Durante el proceso, quizá aparezcan los botones **Keep** y **Undo** en el editor de código. Cuando Copilot termine, tendrás la opción **Keep** o **Undo** para todos los cambios; no es necesario que selecciones ninguna mientras siga trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión de Copilot en modo Agente ya completada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que generan Copilot u otras herramientas de inteligencia artificial (IA).

10. Vuelve al navegador donde está abierto el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

Has explorado cómo usar GitHub Copilot en modo Agente para añadir nuevas funciones al sitio web de Tailspin Toys. Has aprendido cómo:

- Copilot en modo Agente ha explorado el proyecto, identificado los archivos pertinentes y coordinado cambios entre ellos;
- Copilot en modo Agente ha implementado funciones nuevas en la interfaz de usuario y la capa de datos;
- has revisado los cambios y las pruebas generados por Copilot antes de integrar el trabajo en la base de código.

Ahora, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y crearás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que añada la paginación al sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación: por ejemplo, si permites que los usuarios seleccionen el tamaño de página o si lo estableces de forma fija;
- al redactar la solicitud, proporciona a Copilot las indicaciones necesarias para implementar la solución que quieres;
- quizá tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es normal al trabajar con Copilot.

## Recursos

- [Modos de pregunta, edición y agente de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo Agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
