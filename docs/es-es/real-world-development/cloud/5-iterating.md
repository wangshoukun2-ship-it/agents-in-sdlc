---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido a Copilot que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar sobre ella e implemente mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Vamos a revisar los cambios de código y, si hace falta, dar indicaciones a Copilot para mejorar su trabajo.

### Escenario

Como ya hemos comentado varias veces, los fundamentos del diseño de software y DevOps no cambian al incorporar la IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con eso en mente, revisemos las propuestas de GitHub Copilot para añadir documentación, crear la función de juegos relacionados y mejorar la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y el agente de GitHub Copilot en la nube

Como el agente de GitHub Copilot en la nube realiza sus tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para garantizar que todo se mantenga protegido. Por ejemplo:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) generada por el agente de GitHub Copilot en la nube: añadir documentación al código. Para ello, usaremos la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al revisar la PR, puede que veas una advertencia que indica que el cortafuegos ha bloqueado a GitHub Copilot. **Es normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente de GitHub Copilot en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud de incorporación de cambios titulada, por ejemplo, **Add missing documentation** o algo parecido.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes aprovechar para descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Mientras revisas los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso que indica que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. Este proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. Puedes mencionar a Copilot en los comentarios de la solicitud, igual que harías con otros miembros del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendar un texto concreto. Puedes pedirle, por ejemplo:

- que añada encabezados de comentario al principio de cada archivo de código con una breve descripción de su función;
- que añada comentarios TSDoc a los archivos TypeScript y Astro;
- que cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Puedes usar una de las ideas anteriores o pedirle otra mejora de documentación que quieras ver en el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para hacer las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer un nuevo commit en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la solicitud creada por Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un aviso que indica que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. Este proceso puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en el codespace para probar manualmente la función de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube para solicitar cambios. Por ejemplo, puedes pedirle que ajuste cuántos juegos relacionados se muestran o que añada encabezados de comentarios y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de que actualizaras las instrucciones personalizadas). Para ello, añade un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las mejoras de accesibilidad

Por último, revisemos las mejoras de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde el panel de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias del usuario;
   - los cambios de CSS o estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.);
   - el código JavaScript o TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso que indica que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. Este proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en el codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador para comprobar que funcionan como se espera y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora de accesibilidad, puedes mencionar a **@copilot** en un comentario para solicitar cambios, igual que hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta más útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otras tareas. Para acostumbrarte a trabajar así, registra unas cuantas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para que las personas interesadas apoyen un juego en la página de detalles;
- implementar la paginación en la página de lista de juegos;
- añadir validación de entradas y gestión de errores a los asistentes de acceso a los datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para observar después su efecto en el trabajo del agente en la nube.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo prepara su entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y próximos pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente en el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` con el agente en la nube.

Prueba estas ideas para seguir trabajando en tu propio repositorio:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[previous-lesson]: ../4-managing-agents/
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
