---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera un borrador de solicitud de incorporación de cambios para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con el agente de GitHub Copilot en la nube explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Para empezar, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida; solo algo que pueda usar para las pruebas de aceptación de la experiencia de usuario. Aunque esta tarea bloquea el trabajo, hay otras incidencias que ahora mismo tienen más prioridad.

Estas son tareas que suelen perder prioridad rápidamente y que encajan muy bien con el agente de GitHub Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el trabajo de Copilot para comprobar que todo está como esperaba.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Igual que cuando se trabaja con una persona, hay varias formas de asignarle tareas; por ejemplo, [asignarle una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot creará un borrador de solicitud de incorporación de cambios para seguir el progreso, configurará un entorno y empezará a trabajar. Puedes consultar la sesión de Copilot mientras sigue en curso o una vez terminada. Cuando la solución propuesta esté lista para que la revises, te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de pensar. Incluso las tareas que parecen sencillas pueden ser complejas cuando se analizan en detalle, así que [define con cuidado el alcance de las tareas para el agente de GitHub Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación basado en IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar la IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de GitHub Copilot en la nube, revisa los archivos de instrucciones que ya incluye este repositorio, sin modificarlos, para que después puedas observar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes a Copilot la incidencia `Falta documentación en el código` en la siguiente sección, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios del código en la solicitud de incorporación de cambios resultante. Estas reglas proceden de los archivos de instrucciones; volveremos a mencionarlas al revisar las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de GitHub Copilot en la nube

Crear código suele requerir un entorno específico y algunos scripts de configuración para garantizar que todo esté preparado, independientemente de quién realice el trabajo. Esto también se aplica a las tareas que se asignan a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá acceso a las herramientas de desarrollo y dependencias necesarias. Este flujo ya está configurado para que el laboratorio avance sin problemas y puedas aprender cómo funciona. Garantiza que Copilot tenga acceso a Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y sembrada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (ahora toda la aplicación está desarrollada con Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y siembra la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente, sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores al equipo. Por suerte, Copilot destaca a la hora de crear documentación, así que esta es una incidencia ideal para asignarle al agente de GitHub Copilot en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás para revisar su trabajo.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Falta documentación en el código` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Nuestra organización requiere que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación cuando aclaren el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar. Su título será parecido a `[WIP]: Falta documentación en el código`. Si aún no aparece en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la solicitud de incorporación de cambios. Copilot está preparando su entorno (ejecutando el flujo de trabajo de configuración mencionado antes), analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan adecuado, y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente de GitHub Copilot en la nube es que puede realizar tareas de forma asíncrona, lo que te permite centrarte en otras cosas. Aprovecharemos esa posibilidad: asignaremos otra tarea al agente y, mientras tanto, nos ocuparemos de escribir código para añadir funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente de GitHub Copilot en la nube es que permite dividir el trabajo: puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar mucho tiempo, puedes dedicar ese tiempo a otras tareas. ¡Vamos a asignársela al agente en la nube!

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Mostrar juegos relacionados en la página de detalles del juego` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir un asistente de acceso a datos en `src/lib/` que devuelva los demás juegos de la misma categoría que uno determinado, excluido ese juego
   - Mostrar en la página de detalles una sección «Juegos relacionados» que use el asistente
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el asistente nuevo
   - Antes de crear la PR, asegurarse de que se superan todas las pruebas
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás varios ojos 👀 en el primer comentario de la incidencia. Eso indica que Copilot ya está trabajando.

![Copilot indica con el emoji de ojos que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube funciona de forma parecida a un ingeniero de software, así que no hace falta que lo supervises continuamente; basta con revisar el resultado cuando termine. Ahora veremos cómo crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias a Copilot para que trabaje en ellas de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y usar agentes personalizados. [El agente de GitHub Copilot en la nube también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que hemos visto en módulos anteriores.

## Recursos

- [Información sobre el agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
