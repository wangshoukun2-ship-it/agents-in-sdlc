---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo completo en Tailspin Toys. En concreto:

- has preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado precargada;
- has añadido las valoraciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, lo has implementado con Autopilot y lo has revisado en el modo Interactive;
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y la has usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar el filtrado en un navegador real;
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- has revisado todos los cambios del filtrado y autorizado a Agent Merge para combinar la PR;
- has explorado comandos de barra para gestionar el contexto, los modelos y el uso compartido, y para delegar trabajo opcionalmente al agente en la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones con estrellas**: muestra `starRating` cuando existe y un estado explícito para los juegos sin valorar.
2. **Flujo de filtrado y calidad**: implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has trabajado con la misma conversación y rama. Para simplificar el taller, hemos incluido todo ese trabajo en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, examinándolo en el navegador y usando Copilot para explorar el sitio con MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, y presentó los resultados en el formato nuevo. Antes de la PR, QA reunió esos resultados con una revisión de los requisitos y la cobertura de pruebas.

Las pruebas nuevas deben cubrir carencias reales; puede ser correcto que una ejecución de QA no necesite ninguna prueba nueva. Antes de autorizar la combinación, revisa el código y las pruebas, y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Puedes reutilizar estas personalizaciones en distintas conversaciones y ajustarlas según las necesidades del equipo. Las instrucciones definen estándares; las habilidades, tareas que se pueden repetir; los agentes personalizados, funciones especializadas; y los servidores MCP, conexiones a herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para estudiar el enfoque antes de empezar, **Interactive** para seguir de cerca los cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye mucho en el resultado.

## Más cosas que puedes explorar

Ya has completado el flujo de trabajo principal. Estas funciones adicionales de la CLI pueden resultar útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años pensando en desarrollar. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
