---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto, pero los cambios de mayor alcance requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con el flujo de trabajo de una organización y ayudarnos a desarrollar lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual basado en agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que funciona como se espera, y finalmente lo combinarás correctamente en el proyecto.

> [!NOTE]
> Seguirás trabajando en la funcionalidad con la misma conversación y rama. Normalmente se usarían ramas o PR diferentes para los distintos tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- comprobarás manualmente la funcionalidad en un navegador reenviado.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la posibilidad de combinar categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Parten de una incidencia registrada que describe el trabajo necesario.
2. Preparan un plan con lo que hay que desarrollar.
3. Desarrollan y revisan el código.
4. Ejecutan pruebas para validar el código.
5. Comprueban manualmente la nueva funcionalidad.
6. Crean una solicitud de incorporación de cambios (PR).
7. Cuando se ha revisado el código y se ha superado el proceso de integración continua, combinan el código.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen alguna variante de este modelo.

Al seguir este proceso estándar, te aseguras de que el código generado por la IA cumple los requisitos y pasa por las mismas comprobaciones que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Propone cambios y espera tus indicaciones para continuar.
- **Plan**: primero prepara un plan y no puede editar los archivos del proyecto.
- **Autopilot**: trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta considerar que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve al codespace y comprueba que el repositorio y el terminal estén preparados.

1. Vuelve al codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de la valoración con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de encargársela a Copilot. Siempre es recomendable dedicar un rato a planificar las tareas complejas. Cambia al modo Plan y pídele a Copilot que prepare el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este prompt:

   ```plaintext
   Prepara un plan para implementar esta funcionalidad.
   ```

Copilot empezará a trabajar en el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Puede que Copilot te haga preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias. No hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique las partes que no reflejen tus decisiones.

## Aprueba Autopilot

Ya has escrito y revisado el plan; ahora toca implementarlo. Deja que Copilot trabaje en la tarea con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, puede que la implementación empiece inmediatamente. Revísalo primero. Si Copilot informa de dependencias que faltan o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y comprueba la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador del modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones que has tomado durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

De acuerdo, el código parece correcto, pero ¿funciona? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la función de filtrado en el navegador. Indícame la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editor y las combinaciones que acordaste en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y próximos pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la funcionalidad con Autopilot;
- has revisado el código;
- has comprobado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizaremos en cómo generar código de acuerdo con las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
