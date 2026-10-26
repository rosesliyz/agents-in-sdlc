---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, desarrollara una función de juegos relacionados para que el equipo de diseño pudiera probarla e implementara funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios en el código y, si hace falta, envía comentarios a Copilot para mejorar su trabajo.

### Escenario

Como ya se ha indicado varias veces, los fundamentos del diseño de software y DevOps no cambian con la incorporación de la IA generativa. Siempre hay que revisar el código generado y seguir el proceso DevOps habitual. Con esto en mente, revisa las propuestas de GitHub Copilot para la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar las revisiones para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para el código.
- El agente en la nube se ejecuta dentro de GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Protocolo de contexto de modelo (MCP).

## Revisar la documentación generada

La primera solicitud de incorporación de cambios (PR) que haya generado el agente en la nube de GitHub Copilot añade documentación al código. La revisarás con la interfaz habitual de solicitudes de incorporación de cambios de GitHub.com.

> [!NOTE]
> Al revisar la solicitud, puede que veas un aviso que indica que un cortafuegos ha bloqueado a GitHub Copilot. Es **esperable**, ya que Copilot tiene acceso limitado a los recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada algo parecido a **Add missing documentation** o con un título más descriptivo. Si Copilot sigue trabajando y aparece la marca **[WIP]**, espera a que termine.

> [!NOTE]
> Si aparece la marca **[WIP]**, Copilot sigue trabajando en la tarea. Puede tardar unos minutos; mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye nuevos comentarios de documentación TSDoc y otros elementos de documentación. Los cambios exactos pueden variar.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo están a la espera de aprobación.
8. Si hay flujos de trabajo a la espera, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la cola de la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. Puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no tiene por qué ser unidireccional. Puedes mencionar a Copilot en comentarios de la solicitud o en comentarios insertados en el código, como harías con otras personas del equipo. Copilot verá esos comentarios e iniciará otra sesión para abordarlos. Como los resultados no son deterministas, no podemos darte un texto exacto que debas usar. Puedes pedirle, por ejemplo, que:

- añada al principio de cada archivo de código un encabezado con una descripción breve;
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro;
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario en el que solicites un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier persona. Puedes usar una de las ideas anteriores o proponer otra mejora para la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado los cambios, debería aparecer una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

6. Si quieres pedir más cambios, sigue iterando hasta que estés conforme. Después, marca la solicitud como lista para revisión y combínala en la rama principal.

    ![Marcar la solicitud de incorporación de cambios como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

La siguiente solicitud corresponde a la incidencia que pide mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page** o uno más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que algunos flujos de trabajo están a la espera de aprobación.
7. Si hay flujos de trabajo a la espera, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la cola de la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. Puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama desde tu codespace para probar manualmente la función de juegos relacionados. Ve al codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. También puedes trabajar de forma iterativa con el agente en la nube para pedirle cambios.

10. Si quieres ajustar la función, añade un comentario en la pestaña **Conversation** y pide a Copilot que cambie, por ejemplo, el número de juegos relacionados o que añada encabezados de comentarios y comentarios de documentación TSDoc. Recuerda que esta tarea se asignó **antes** de que actualizaras las instrucciones personalizadas. Copilot verá el comentario e iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

La última solicitud de Copilot que revisarás corresponde a las funciones de accesibilidad implementadas con el agente personalizado. Debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website** o uno más descriptivo. Si Copilot sigue trabajando y aparece la marca **[WIP]**, espera a que termine.

> [!NOTE]
> Si aparece la marca **[WIP]**, Copilot sigue trabajando en la tarea. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   1. los componentes de interfaz para cambiar de modo;
   2. el uso del almacenamiento local para conservar las preferencias;
   3. los cambios de hojas de estilo en cascada (CSS) para los modos de alto contraste y claro;
   4. los atributos de accesibilidad, como las etiquetas de Accessible Rich Internet Applications (ARIA) y la navegación con teclado;
   5. el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo están a la espera de aprobación.
8. Si hay flujos de trabajo a la espera, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la cola de la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. Puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama desde tu codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que creó Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba en el navegador los modos de alto contraste y claro. Comprueba que funcionan y que la selección se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ayudó a orientar a Copilot para implementar estas funciones siguiendo prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, sigue estos pasos:

1. Añade un comentario con **@copilot** para solicitar los cambios, como hiciste con las solicitudes anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta especialmente útil cuando puedes asignarle tareas reales de la lista de pendientes y ocuparte de otras cosas mientras trabaja. Algunas tareas que puedes delegar mediante incidencias adicionales son:

1. Crea un formulario de interés para mecenas en la página de detalles de los juegos.
2. Implementa la paginación en la página de la lista de juegos.
3. Añade validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el entorno de trabajo del agente en la nube. A lo largo de estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas que incluye este repositorio** para ver su efecto posterior en los resultados del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para tareas de accesibilidad, con modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno de trabajo del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado aquí:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora Copilot Chat en modo Agente y la integración de MCP directamente desde el IDE.
- 💻 **[Entorno de interfaz de línea de comandos (CLI)](../../cli/)**: implementa dos cambios revisados desde el terminal con los modos Plan y Autopilot, skills de agente, un agente personalizado de control de calidad, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

1. Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
2. Crea un agente personalizado para otro ámbito.
3. Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
