---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. Hay que trabajar con datos y servicios externos, e incluso habilitar automatizaciones adicionales para Copilot. Para eso sirven los servidores MCP: permiten que Copilot use herramientas y servicios que no están integrados en la CLI.

En esta lección:

- aprenderás qué es el Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadirás el servidor Playwright MCP si aún no está disponible;
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios en la interfaz de usuario hay que interactuar con ella. Quieres que Copilot use el sitio web como lo haría una persona y automatice más el proceso para aumentar la confianza en que los cambios funcionan como se espera.

## ¿Qué es el Model Context Protocol (MCP)?

El [Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que el agente puede acceder.

Algunos servidores MCP conocidos:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: proporciona funciones de automatización del navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP][mcp-registry] para facilitar que se descubran y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta sus implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve al codespace.
2. En Copilot CLI, ejecuta el siguiente comando para abrir el cuadro de diálogo para añadir servidores MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Pulsa <kbd>Enter</kbd> para confirmar el tipo de servidor **STDIO** y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP nuevo.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes has comprobado manualmente que la funcionalidad funciona como esperabas. Ahora hagamos lo mismo con el servidor Playwright MCP disponible en Copilot.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad con el servidor Playwright MCP:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar que el filtrado cumple la incidencia y el plan. Indícame qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que has iniciado.
   ```

> [!NOTE]
> No es necesario indicar a Copilot que use el servidor MCP; normalmente lo deducirá por sí mismo. Aun así, como sabes qué debería usar, conviene orientarlo. Esto ayuda a obtener resultados más coherentes y ahorra algunos tokens.

2. Observa cómo Copilot enumera los distintos pasos que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te entregará un informe.

## Resumen y próximos pasos

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar tu funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es el Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- has añadido el servidor Playwright MCP si aún no estaba disponible;
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en una función especializada.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
