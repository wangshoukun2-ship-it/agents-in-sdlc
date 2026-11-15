---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar cada diff, aceptar o rechazar los cambios individualmente y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- orientarás al agente durante la sesión para añadir un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has usado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha hecho una solicitud adicional: también quiere un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de iniciar una conversación nueva, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada modificación de archivo aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramienta. Verás lecturas y ediciones de archivos, y los comandos del terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerá el diff en el chat:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Orienta al agente para añadir el modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación con una opción de modo claro. Como continuarás en el mismo chat, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde el campo de Copilot Chat:

    ```plaintext
    Buen trabajo. Ahora añade un modo claro junto al modo de alto contraste. Debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo de alto contraste y el modo claro de forma independiente.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios anteriores y propondrá modificaciones para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que hiciste con el modo de alto contraste. Selecciona **Keep** o **Undo** para cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de alternancia de una forma que no te gusta—, detenlo con el botón **Stop** junto al campo de chat y envía un mensaje de seguimiento que aclare lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa los prompts de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los archivos modificados

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa sus diffs. Confirma que se siguen las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición cuando aparecía, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado en tiempo real el trabajo del agente;
- has aceptado y rechazado ediciones de archivos individuales;
- has orientado una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
