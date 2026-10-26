---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones —si es que hay alguna— no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en la lista de tareas pendientes porque no había tiempo para implementarlas. El agente en la nube de GitHub Copilot está pensado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones más importantes.

Con el agente en la nube de Copilot explorarás cómo:

- personalizar el entorno para generar código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere abordar. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco adecuado: como verás, no hay ninguna. Para empezar, quieren añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría, para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida: solo algo que le permita probar la experiencia de usuario (UX). Aunque esto bloquea el trabajo, hay otras incidencias de mayor prioridad.

Son tareas que pueden quedar relegadas con facilidad y que conviene asignar al agente en la nube de Copilot. Este puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas; después, podrás revisar su trabajo y comprobar que todo esté como esperas.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a una persona desarrolladora. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una solicitud de incorporación de cambios en borrador para hacer un seguimiento del trabajo, prepara un entorno y empieza la tarea. Puedes consultar la sesión mientras sigue activa o cuando haya terminado. Cuando la solución esté lista para que la revises, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las tareas que parecen sencillas se complican cuando se analizan en detalle, así que [define con cuidado el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Úsalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La incorporación de la IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has realizado un ejercicio práctico sobre ellas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones de solo lectura que ya incluye este repositorio para reconocer más adelante su efecto.

Abre los siguientes archivos en la interfaz web de GitHub de tu repositorio o en un codespace si ya tienes uno en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en particular las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que aplican estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, comprueba si la solicitud de incorporación de cambios incluye comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones. Volverás a revisarlas al examinar las solicitudes de incorporación de cambios en una lección posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

Crear código suele requerir un entorno concreto y ejecutar algunos scripts de configuración para que todo esté listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo especial de configuración][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para facilitar el taller y ofrecerte esta oportunidad de aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada para la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar estos pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos que necesiten tus pasos. Copilot recibirá un token propio para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (la aplicación completa ahora usa Astro + Drizzle/libSQL).
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

      # Migra y puebla la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos puntos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar pendiente, ralentiza el trabajo y dificulta el mantenimiento de la base de código o la incorporación de nuevas personas al equipo. Copilot puede ser especialmente útil para crear documentación, por lo que esta es una buena incidencia para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior revisarás su trabajo.

La primera tarea consiste en crear y asignar una incidencia nueva en tu repositorio:

1. Abre tu repositorio en GitHub.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias nuevas.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para asegurarnos de que se incluyan comentarios de documentación cuando aclaren el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece ninguna, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas pendientes.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la solicitud de incorporación de cambios. Copilot está preparando su entorno (ejecutando el flujo de trabajo descrito antes), analizando el proyecto y decidiendo cómo abordar el problema.

13. Revisa la lista y las tareas que Copilot va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**. Si el indicador no se actualiza, actualiza la ventana del navegador.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que el indicador tarde en actualizarse.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código, analiza el estado, se detiene a planificar y tomar decisiones, y crea código.

Es probable que esto tarde varios minutos. Una de las ventajas principales del agente en la nube de Copilot es que puede trabajar de forma asíncrona y dejarte centrarte en otras tareas. Aprovecharás esta característica asignándole otra tarea y, mientras tanto, escribirás código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. La siguiente tarea consiste en asignarle esta función al agente en la nube de Copilot.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias nuevas.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    Queremos ayudar a la gente a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añade una función auxiliar de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, sin incluir ese juego.
   - Muestra en la página de detalles del juego una sección «Juegos relacionados» que use la función auxiliar.
   - Gestiona el caso en que un juego no tenga juegos relacionados.
   - Crea pruebas unitarias para la nueva función auxiliar.
   - Antes de crear la PR, comprueba que todas las pruebas se ejecuten correctamente.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás un conjunto de ojos 👀 en el primer comentario de la incidencia, que indica que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de forma parecida a un ingeniero de software: no necesitas supervisarlo continuamente, sino revisar el resultado cuando termine. Ahora puedes centrarte en crear y usar agentes personalizados.

## Resumen y siguientes pasos

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona. Así, Copilot puede ayudar a resolver deuda técnica, crear funcionalidades o facilitar la migración de código de un framework a otro.

- Has personalizado el entorno para generar código.
- Has comprobado cómo garantizar que las operaciones se realicen de forma segura.
- Has definido incidencias con un alcance claro.
- Has asignado incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes centrarte en crear y usar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores del Protocolo de contexto de modelo (MCP)][cloud-agent-mcp] y admite instrucciones personalizadas, que ya has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

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
