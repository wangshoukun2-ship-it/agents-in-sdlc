---
title: "Ejercicio 2 - Añade funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen requerir modificar varios archivos y realizar operaciones como ejecutar pruebas. Como desarrollador, tu flujo de trabajo suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado, actualizarlo... Y así sucesivamente.

Para eso está el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres son parecidos, el modo de agente y el agente en la nube están diseñados para experiencias distintas. El modo de agente trabaja en el IDE y permite obtener comentarios e interactuar rápidamente. El agente en la nube está diseñado para trabajar de forma asíncrona como un compañero del equipo, e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- el modo de agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya has añadido un asistente para editores. Ahora completarás el resto del trabajo de la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas en el momento de la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz de cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Inicia el sitio web

Para facilitar la ejecución del sitio web, se incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces siguiendo estos pasos:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre una ventana nueva del terminal en Codespaces con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal para abrir el sitio.

> [!NOTE]
> En un codespace, al seleccionar un enlace `localhost` en el terminal de Codespaces, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, podrás explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

La implementación inicial del sitio funciona, pero queremos ampliarla con nuevas capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el menú desplegable de agentes de Chat. El agente **Agent** planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Al crear el taller, usamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea satisfactoria. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes, Copilot podría usar otro comando MCP, pero debería poder completar la tarea.

3. Pide a Copilot que muestre el backlog con este prompt:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según cuáles vayan a ser más útiles para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado los elementos según cuáles cree que serán más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedirle al agente que genere código, conviene revisar el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para varios tipos de archivo, entre ellos:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante del filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, hay que ejecutar las pruebas y asegurarse de que se superan antes de combinar los cambios en el código base. El modo de agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog con este prompt:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editor y categoría de acuerdo con los requisitos de la incidencia relacionada de GitHub en el backlog. Ya existe un asistente para editores del ejercicio anterior, así que conserva y ajusta ese trabajo cuando sea necesario para completar la capa de datos, la interfaz de usuario y las pruebas restantes. Asegúrate de que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución; no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: tanto los asistentes de la capa de datos y la interfaz de usuario como las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot mientras explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código base. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás que aceptar o deshacer todos los cambios; no hace falta que lo hagas mientras el agente trabaja.

7. Cuando Copilot te lo indique, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide confirmar si quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que el proceso se pause y que algunas pruebas fallen. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de una sesión completa de Copilot Chat en el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Has visto cómo:

- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos;
- el modo de agente puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Aplica lo que has aprendido para pedir a Copilot que la implemente en el sitio. Al planificar el código:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación y si el usuario podrá elegir el tamaño de página o estará fijado;
- proporciona a Copilot las directrices necesarias para crear la implementación que necesitas;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este es el flujo normal cuando se trabaja con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
