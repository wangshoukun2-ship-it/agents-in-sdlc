---
title: "Lección 2 - Añade valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y has probado una conversación, es hora de hacer el primer cambio en el proyecto. Será un cambio pequeño: los juegos ya tienen una valoración con estrellas en sus datos, pero aún no aparece en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación específica de Copilot en una rama de funcionalidad;
- pedirás al agente que haga un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación para comprobarlo en un navegador reenviado;
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en su página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, el editor y la descripción. Para empezar, pedirás al agente que muestre la valoración existente en cada tarjeta. Es un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git actualmente seleccionados en el terminal, en vez de crear un árbol de trabajo dedicado. Las conversaciones guardadas te permiten volver a la misma conversación más adelante, mientras que los archivos y la rama siguen formando parte del estado normal de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas que usa para explorar y editar archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Empecemos una conversación nueva para implementar la funcionalidad.

1. Vuelve al codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está abierto, inicia el agente con este comando:

   ```bash
   copilot --yolo
   ```

4. Comprueba que se inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para solicitar el cambio:

   ```plaintext
   Muestra la valoración de cada juego sobre 5 con `starRating` en las tarjetas de la página de lista. Si la valoración es `null`, muestra exactamente `No rating yet`. Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa el diff

Antes de combinar cualquier cambio generado por IA, incluso uno pequeño, hay que revisarlo. Vamos a inspeccionar los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que las tarjetas muestran la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot ofrece resultados probabilísticos, no deterministas, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pidamos a Copilot que inicie el sitio para que podamos examinar la interfaz de usuario (UI) actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda comprobar el cambio de la valoración con estrellas en el navegador. Indícame la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abre una solicitud de incorporación de cambios (PR) para combinar el código nuevo en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Crea un commit con los cambios revisados de la valoración con estrellas y un mensaje de commit adecuado.
   ```

2. Escribe `/pr create`. Cuando cree la PR, Copilot CLI enviará el commit existente y mostrará la URL de la PR.
3. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve al codespace y sal de Copilot CLI con `/exit`.
7. Actualiza la rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y próximos pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación de Copilot específica en una rama de funcionalidad;
- has indicado al agente que haga un pequeño cambio en las tarjetas de juegos;
- has revisado el cambio con `/diff`;
- has ejecutado la aplicación para comprobar las valoraciones en un navegador reenviado;
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
