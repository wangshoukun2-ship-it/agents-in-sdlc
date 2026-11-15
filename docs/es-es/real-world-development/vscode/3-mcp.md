---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es el Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadirás el servidor Playwright MCP al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente la función de filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es el Model Context Protocol (MCP)?

El modo de agente ofrece muchas más posibilidades cuando puede acceder a recursos externos al editor. El Model Context Protocol (MCP) permite que Copilot lo haga: es un estándar para que el agente se comunique con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

El [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada (mediante recursos) y realizar acciones en tu nombre (mediante herramientas).

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. El servidor MCP gestiona la comunicación entre el agente y las herramientas externas, como las API existentes o las herramientas locales, por ejemplo, los paquetes de NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que el agente puede acceder.

Algunos servidores MCP conocidos:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a varias API para gestionar los repositorios de GitHub. Permite que el agente de IA realice acciones como crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador con Playwright. Permite que el agente de IA realice acciones como abrir páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles, con acceso a distintas herramientas y recursos. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar que se descubran y contribuir al ecosistema.

> [!CAUTION]
> En cuanto a la seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta sus implicaciones de seguridad. Usa únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías ver que ya incluye un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada en local. VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor Playwright MCP

Ahora añadirás otro servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor `github` remoto, `playwright` es un servidor **local**. VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin ventana visible, algo necesario en un codespace, que no tiene un escritorio donde mostrarlo.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para sus pruebas de extremo a extremo, por lo que normalmente ya está instalado el navegador que necesita Playwright. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

El inicio manual es opcional. Si lo omites, VS Code iniciará el servidor (y te pedirá que confirmes que confías en él) la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Prueba la función de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de navegar por la aplicación para comprobar tu trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer el resultado, y después resumir lo que ha observado. Es la forma más rápida de confirmar que una función se comporta como esperas sin salir de la conversación.

Internamente, el servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Así, el agente razona sobre elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como lo hace una tecnología de asistencia. Por eso, una comprobación funcional rápida también permite revisar aspectos básicos de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la función de filtrado que acabas de crear:

```text
Usa el servidor Playwright MCP para abrir un navegador en la aplicación que se está ejecutando en http://localhost:4321 y comprobar la nueva función de filtrado de juegos:

1. Ve a la página de juegos y anota cuántos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
3. Borra el filtro y, después, aplica un filtro de editor. Confirma que la lista se actualiza para mostrar los juegos de ese editor.
4. Combina un filtro de categoría y uno de editor y confirma que los resultados cumplen ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot iniciará un navegador mediante el servidor Playwright MCP, realizará cada paso y te informará de los resultados. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor Playwright MCP, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y lo vuelva a intentar.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has comprobado que la funcionalidad funciona y puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que la cree. Puede localizar la incidencia correspondiente y vincularla a la PR.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en el menú desplegable de agentes, para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editor. Crea una solicitud de incorporación de cambios para la rama actual y vincúlala a la incidencia correcta.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub, quizá te pida que te autentiques; sigue las indicaciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es el Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- has añadido el servidor Playwright MCP al espacio de trabajo;
- lo has usado para probar manualmente la función de filtrado en un navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
