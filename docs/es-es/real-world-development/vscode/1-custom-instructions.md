---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o hay información de fondo que Copilot debe conocer, conviene asegurarse de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que te permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas;
- generarás el primer componente de datos para el filtrado (un asistente para editores) con las instrucciones actuales;
- añadirás un estándar para todo el repositorio en `.github/copilot-instructions.md`;
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar;
- confirmarás los cambios de las instrucciones y el componente de filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la *tendencia* de los cambios en su comportamiento al actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su función.

Con los archivos de instrucciones, te asegurarás de que Copilot dispone de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar a Copilot para que ofrezca sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación y las bibliotecas que prefieres, e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y contexto específico de una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información general del proyecto, es decir, contexto pertinente para la mayoría de las solicitudes de chat o CLI dirigidas a Copilot. Puede describir la pila tecnológica, lo que se está desarrollando, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos concretos. Permiten definir directrices para lenguajes específicos (como TypeScript o Astro) o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni las de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo` en el frontmatter al generar código.
>
> Copilot también [admite archivos de instrucciones con otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Hablar en detalle sobre cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices del proyecto, como una descripción de lo que se desarrolla, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para dar instrucciones específicas sobre tipos de archivos (pruebas unitarias, componentes de Astro o la capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de cómo debería y cómo no debería ser el código.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única forma correcta de usar la IA. La experimentación te permitirá encontrar lo que funciona mejor para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá observes que hay instrucciones para muchos tipos de tareas, como las [actualizaciones de interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente al entorno que estés usando lo indicará cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas sobre **GitHub Actions Workflows**. Estas se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y echa un vistazo. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` del principio: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que encaje con `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir al generar código. Las rutas que aparecen ahí son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas del editor para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el asistente.
3. Selecciona **Agent** en el menú desplegable de agentes de Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un asistente de acceso a datos que devuelva una lista de todos los editores e incluya el nombre y el id de cada uno. Aplica los cambios a los archivos.
   ```

5. Copilot explorará el proyecto y aplicará cambios al código, a menudo tanto al archivo del asistente como a sus pruebas.
6. Observa que el asistente propuesto es una función con tipos que recibe un cliente `db` como primer argumento y devuelve una matriz con tipos de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada; el objetivo es observar la mejora de *coherencia* después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como ya hemos indicado, `.github/copilot-instructions.md` sirve para proporcionar información general del proyecto a Copilot. Documentemos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación del proyecto, pero todavía no incluye reglas para documentar el código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares actuales para indicar a Copilot que añada encabezados de comentario a los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el repositorio (`.github/copilot-instructions.md`) para definir directrices globales, o archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares de todo el proyecto, como la regla sobre los comentarios de documentación que acabas de añadir.

## Vuelve a ejecutar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar la conversación.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un asistente de acceso a datos que devuelva una lista de todos los editores e incluya el nombre y el id de cada uno. Aplica los cambios a los archivos.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Asistentes de acceso a datos de editores para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editores de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su id y nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que se usará en el siguiente ejercicio.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan con el mismo sistema de control de versiones que los demás archivos. Confirmemos los cambios y enviemos la rama.

1. Abre una ventana nueva del terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, ejecuta este comando para comprobar que están los cambios de las instrucciones y del asistente:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos que Copilot haya cambiado para crear la base del asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones de archivos complementarios (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ya has enviado la rama; ahora creemos una solicitud de incorporación de cambios asociada a la incidencia del backlog sobre el estándar de documentación. Podrías hacerlo manualmente, pero Copilot puede encargarse de ello con las herramientas de GitHub conectadas al proyecto. Pídele que encuentre la incidencia, cree una PR para resolverla y, después, la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no hay nada que configurar. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las indicaciones. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de los archivos de instrucciones y cree una PR desde la rama actual que describa tanto la actualización de las instrucciones como la base del asistente de editores:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de incorporación de cambios desde la rama actual, indica que resuelve esa incidencia e incluye que también hemos añadido la base del asistente de editores para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso (**Allow**) para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pídele a Copilot que la combine en `main`, vuelva a esa rama y descargue el código más reciente con este prompt:

   ```
   Combina la PR en main. Después, vuelve localmente a main y descarga el código más reciente para actualizar el repositorio.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar para todo el repositorio en `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto sigue el nuevo estándar;
- confirmar los cambios de las instrucciones y la base del asistente en `main` para que se puedan usar en el siguiente ejercicio.

A continuación, aplicarás estas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad en varios archivos del código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
