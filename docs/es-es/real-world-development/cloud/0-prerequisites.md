---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios sobre el agente en la nube de Copilot, prepare todo lo necesario. Creará su propia copia del repositorio Tailspin Toys e iniciará un [codespace][codespaces] que podrá usar para editar archivos de instrucciones y revisar el trabajo que genere el agente.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio donde trabajará, cree una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios para el laboratorio y la usará durante los ejercicios.

1. En una nueva ventana del navegador, vaya a la [plantilla de Tailspin Toys][tailspin-template].
2. Cree su propia copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, seleccione **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participa en el taller como parte de un evento dirigido por GitHub o Microsoft, siga las instrucciones de los mentores. De lo contrario, puede crear el repositorio nuevo en una organización en la que tenga acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anote la ruta del repositorio que ha creado (**organization-or-user-name/repository-name**); la necesitará más adelante en el laboratorio.

## Crear un codespace

A continuación, usará un codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un entorno de desarrollo integrado (IDE) completo compatible con varios lenguajes de programación, extensiones y herramientas.

1. Vaya al repositorio que acaba de crear.
2. Seleccione el botón verde **Code**.

    ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Seleccione la pestaña **Codespaces** y el botón **+** para crear un codespace. Déjelo abierto en una pestaña del navegador; volverá a usarlo más adelante.

    ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque será mucho más rápido que instalar todos los servicios manualmente. Mientras tanto, puede explorar otras funciones de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverá al codespace en un ejercicio posterior.

> [!NOTE]
> Este taller está diseñado para ejecutarse dentro de un codespace o un [contenedor de desarrollo][dev-containers] local. Ambos proporcionan los requisitos previos necesarios para trabajar sin problemas. Si prefiere trabajar localmente:
>
> 1. Abra el repositorio clonado en VS Code.
> 2. Cuando se le solicite, seleccione **Reopen in Container**. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

Enhorabuena: ha creado una copia del repositorio del laboratorio. También ha iniciado la creación del codespace que usará mientras trabaja con el agente en la nube de Copilot.

## Siguiente paso

Añadamos instrucciones personalizadas que seguirá el agente en la nube. Continúe con el [ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
