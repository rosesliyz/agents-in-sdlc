---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con inteligencia artificial (IA) generativa. Si una tarea debe hacerse de una forma concreta, o Copilot necesita conocer información de fondo, asegúrese de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esta orientación para que Copilot entienda no solo qué debe hacer, sino también cómo debe hacerlo.

En este ejercicio:

- explorará cómo el contexto específico del proyecto, las pautas de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas;
- añadirá un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y de la interfaz de línea de comandos (CLI), aquí no ejecutará una solicitud *antes y después*. El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, por lo que resulta más difícil mostrar el efecto en tiempo real y compararlo directamente. Más adelante, al revisar las solicitudes de incorporación de cambios (PR) que genere el agente, verá la influencia del archivo de instrucciones.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, proporcionará a Copilot la información necesaria para que trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor su estilo de programación y sus requisitos. Puede especificar convenciones de código, bibliotecas e incluso el tipo de comentarios que prefiere. Puede crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que resulte pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está creando, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo específicos. Puede usarlos para proporcionar pautas para lenguajes concretos, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan metadatos `applyTo`.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], incluidos `AGENTS.md` y `CLAUDE.md`.

### Prácticas recomendadas para administrar archivos de instrucciones

Esta guía no abarca en profundidad la creación de archivos de instrucciones, pero el proyecto de ejemplo muestra un enfoque representativo. En general:

- Centre `copilot-instructions.md` en pautas generales del proyecto, como qué se está creando, su estructura y los estándares globales de programación.
- Use archivos `*.instructions.md` para indicar requisitos específicos de tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas concretas.
- Use lenguaje natural y pautas claras. Incluya ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma correcta de crear archivos de instrucciones ni de usar la IA. La experimentación le permitirá descubrir qué funciona mejor para su proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. Al explorar los de este proyecto, verá archivos para distintos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarle a generar archivos de instrucciones. Cada entorno lo ofrece de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno correspondiente lo indicará cuando proceda.
>
> ¿Busca plantillas o un punto de partida? Explore [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Revise los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábralos en el editor o en la interfaz web de GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore el archivo y observe la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observe las pautas anidadas de **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abra la carpeta `.github/instructions` y explore su contenido. Observe las instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe el campo `applyTo` de la parte superior, que define un patrón glob relativo a la raíz del repositorio. En este caso, coincide con cualquier archivo de pruebas TypeScript (por ejemplo, los que cumplen `**/*.test.ts`).
5. Revise las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abra `.github/instructions/drizzle.instructions.md` y desplácese hasta el final. Observe los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Esto permite dividir conjuntos de instrucciones grandes en archivos pequeños y reutilizables, y enlazar ejemplos que Copilot puede seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirá reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Añadir un nuevo estándar para el repositorio

Este será el único cambio que hará en este ejercicio: añadirá una regla general para que la documentación del código se escriba como comentarios TSDoc y encabezados de comentarios a nivel de archivo. El agente en la nube aplicará estas pautas a las incidencias que le asigne más adelante.

Antes de editar, cree una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero usted confirmará los cambios de forma ordenada:

1. En el terminal del codespace, cree una rama y cámbiese a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se indicó antes, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añadamos una regla a los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelva a abrir `.github/copilot-instructions.md`.
2. Busque la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero aún no incluye documentación dentro del código; por eso el ayudante generado no tenía comentarios de documentación.
3. Añada las siguientes líneas de Markdown justo después de los estándares existentes. Indican a Copilot que incluya encabezados de comentarios de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarde `copilot-instructions.md`.

> [!TIP]
> Como vio en la lección anterior, los archivos de instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar pautas globales, o ser archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acaba de añadir.

## Confirmar, enviar e integrar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio asignará una incidencia a Copilot, que creará una rama a partir de `main`; por eso los cambios de instrucciones deben llegar a `main` para que el agente los use.

1. Prepare y confirme los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envíe la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En GitHub.com, abra una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` e intégrela. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asigne en el siguiente ejercicio.

> [!TIP]
> Si prefiere trabajar directamente en `main` durante este taller, puede omitir el paso de crear una rama y confirmar los cambios directamente en `main`. La rama permite practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y siguientes pasos

Ha explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y ha añadido un estándar general a `.github/copilot-instructions.md`. Más adelante, verá cómo influye este estándar en las solicitudes de incorporación de cambios que genere el agente en la nube.

A continuación, [asignará su primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
