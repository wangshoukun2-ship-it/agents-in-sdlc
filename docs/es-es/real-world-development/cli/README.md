---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Desarrolla, verifica y entrega dos cambios de Tailspin Toys mientras aprendes los modos, las personalizaciones, las herramientas MCP y la automatización de solicitudes de incorporación de cambios de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot al terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos. Así puedes mantener el flujo de trabajo sin cambiar a un editor gráfico.

El taller sigue un único flujo de trabajo continuo en Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y familiarízate con el entorno.
2. Añade una valoración con estrellas, revisa el cambio en el navegador y combina manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en el modo Plan, implementa la funcionalidad con Autopilot y revísala con Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al filtrado.
5. Personaliza la habilidad existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa todos los cambios del filtrado y usa Agent Merge para combinar la PR.
9. Explora comandos de barra útiles para el contexto, los modelos y el uso compartido, y la delegación opcional al agente en la nube.

Para centrar el taller, crearás dos PR: primero la valoración con estrellas y, después, el filtrado con las actualizaciones de instrucciones, habilidad, perfil de QA y pruebas. El filtrado y el flujo de calidad comparten una conversación y una rama para que puedas aprovechar el trabajo al explorar cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----- | ---------------- |
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el codespace |
| [1. Instala Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI, y familiarízate con el entorno |
| [2. Añade valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el estado sin valoración, y combina tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad desde su incidencia, impleméntala con Autopilot y revísala en el modo Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones, y aplícalas al filtrado |
| [5. Personaliza y usa la habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en la CLI e inspecciona el comportamiento del filtrado |
| [7. Crea y usa un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas finales de verificación |
| [8. Crea y combina la PR de la funcionalidad][ex8] | Revisión y combinación | Revisa todos los cambios, crea la PR y usa Agent Merge |
| [9. Comandos de barra de GitHub Copilot CLI][ex9] | Funciones de la CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube |
| [10. Repaso y próximos pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: incorpora Foundry][foundry] | Agentes alojados | Prepara un modelo, despliega un agente basado en un catálogo y conéctalo al sitio en tres módulos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un codespace
- [ ] Conocimientos básicos sobre el terminal o la línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se usan en este taller.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, comprueba que el administrador haya habilitado Copilot CLI.

## Primeros pasos

**[Empieza por los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
