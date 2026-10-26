---
slug: real-world-development/cloud
title: "Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

El [**agente en la nube de GitHub Copilot**](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) permite que GitHub Copilot trabaje de forma asíncrona en la nube. El trabajo se asigna en GitHub y el agente en la nube lo realiza en segundo plano: explora el repositorio, hace cambios y abre una solicitud de incorporación de cambios mientras usted puede dedicarse a otras tareas.

En estos ejercicios añadirá instrucciones personalizadas que seguirá el agente en la nube y, después, le asignará una incidencia de GitHub para que implemente el trabajo. Revisará y usará agentes personalizados para orientar su enfoque, supervisará y dirigirá sesiones desde el panel de agentes y terminará revisando las solicitudes de incorporación de cambios e iterando sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Cree el repositorio y un codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añada instrucciones personalizadas que seguirá el agente en la nube |
| [2. Agente en la nube][ex2] | Agente asíncrono | Asigne incidencias al agente en la nube de Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise y use agentes personalizados |
| [4. Administración de agentes][ex4] | Supervisión | Supervise y dirija las sesiones del agente |
| [5. Iteración][ex5] | Revisión | Revise las solicitudes de incorporación de cambios, itere sobre el trabajo de Copilot y elija los siguientes pasos |

## Requisitos previos

Antes de asistir a este taller, asegúrese de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] El agente en la nube de Copilot habilitado para su cuenta u organización

> [!TIP]
> ¿No tiene un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, el Protocolo de contexto de modelo (MCP), la revisión de código y las funciones de Copilot CLI que se usan en este taller, por lo que podrá completar todos los entornos con ese plan.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> El agente en la nube está disponible en **Copilot Student, Pro, Pro+, Business y Enterprise**; en todos los planes excepto Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este entorno no cubre MCP. Para explorar el uso de servidores MCP con Copilot, consulte el [entorno de CLI](../cli/) o el [entorno de VS Code](../vscode/).

## Empezar

**[Comience con el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
