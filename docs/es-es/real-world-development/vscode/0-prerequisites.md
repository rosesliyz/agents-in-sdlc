---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] para trabajar y comprobarás que GitHub Copilot Chat funciona en el editor.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio en la que trabajarás, crea una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios para el laboratorio y la usarás durante los ejercicios.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en el taller como parte de un evento organizado por GitHub o Microsoft, sigue las instrucciones de las personas que lo guían. De lo contrario, puedes crear el repositorio nuevo en una organización en la que tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante en el laboratorio.

> [!NOTE]
> **La lista de tareas pendientes está preparada**
>
> Al crear el repositorio desde la plantilla, se genera automáticamente una lista de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

## Crear un codespace

A continuación, usarás un codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un entorno de desarrollo integrado (IDE) completo compatible con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y el botón **+** para crear un codespace. Déjalo abierto en una pestaña del navegador; lo volverás a usar más adelante. Mientras se crea, puedes explorar otras funciones de GitHub Copilot.

    ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque será mucho más rápido que instalar todos los servicios manualmente.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos proporcionan los requisitos previos necesarios para trabajar sin problemas. Si prefieres trabajar localmente:
>
> 1. Abre el repositorio clonado en VS Code.
> 2. Cuando se te solicite, selecciona **Reopen in Container**. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar GitHub Copilot Chat y el modo Agente

Para usar el modo Agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Si usas GitHub Codespaces, ya debería estar instalada.

Después de instalar la extensión, quizá tengas que autenticarte con tu cuenta de GitHub para habilitarla. Sigue estos pasos para comprobar que Copilot Chat funciona:

1. Vuelve al codespace.
2. Si no tienes instalada la extensión GitHub Copilot Chat, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension] o abre la vista Extensiones en Visual Studio Code, busca **GitHub Copilot Chat** y selecciona **Install**.
3. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
4. Escribe un mensaje como «Hola, mundo» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Esto debería activar Copilot Chat.
5. Si no has iniciado sesión, se te pedirá que lo hagas con tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo de la solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

6. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

Enhorabuena: has creado una copia del repositorio del laboratorio. También has iniciado la creación del codespace que usarás cuando empieces a escribir código.

## Siguiente paso

Empezarás a poner Copilot a trabajar. Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones de tu proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
