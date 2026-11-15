---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones que han realizado e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se ha dado cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo una función de alternancia, parecía lógico incluirlo. Quieres orientar el trabajo del agente para asegurarte de que, además del modo de alto contraste, también añada un modo claro.

## Revisa las tareas del agente de GitHub Copilot en la nube

Veamos el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, como:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios (PR) de la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, vamos a pedir a Copilot que añada una opción de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya usado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurarse y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo que permita a los usuarios seleccionar el modo de visualización que prefieran.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa que Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso en el que estuviera trabajando al recibir el mensaje.

Como en los ejercicios anteriores, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes seguir todas las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir una tarea en curso para cambiar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
