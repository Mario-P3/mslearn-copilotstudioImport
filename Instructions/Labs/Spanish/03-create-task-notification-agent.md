---
lab:
  title: Crear un agente y un flujo de trabajo de notificación de tareas
  module: Automatizar las tareas del agente con flujos de trabajo en Microsoft Copilot Studio
  description: Crea un flujo de trabajo que envíe correos electrónicos de notificación de tareas, agrégalo a un agente como una herramienta y prueba las rutas de éxito, información faltante, cancelación y error.
  duration: 55 minutos
  level: 200
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
    - Workflows
---

# Crear un agente y un flujo de trabajo de notificación de tareas

## Escenario

En este ejercicio, crearás el **Agente de alertas de tareas de Contoso**, un flujo de trabajo independiente que envía correos electrónicos de notificación de tareas, agregarás el flujo de trabajo al agente como una herramienta con confirmación del usuario final y ejecutarás las cuatro rutas de prueba requeridas.

Completar este ejercicio toma aproximadamente **55 minutos**.

## Lo que aprenderás

- Cómo crear un flujo de trabajo independiente para un agente.
- Cómo definir las entradas y salidas del flujo de trabajo.
- Cómo agregar y configurar un flujo de trabajo como herramienta de un agente.
- Cómo probar las rutas de éxito, información faltante, cancelación y error.

## Pasos generales del laboratorio

- Crear y configurar un agente.
- Compilar y publicar un flujo de trabajo de notificación de tareas.
- Agregar el flujo de trabajo al agente como una herramienta.
- Probar el agente y el flujo de trabajo.
- Revisar la configuración y limpiar los recursos.

## Requisitos previos

Para completar este ejercicio, necesitas:

- Una cuenta profesional o educativa de Microsoft Entra.
- [Acceso a Microsoft Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/requirements-licensing-subscriptions).
- Permiso para crear un agente en un entorno de Power Platform.
- Una cuenta de Microsoft 365 con un buzón de Outlook. La conexión de Outlook usa esta cuenta para enviar correos electrónicos.
- Acceso a una solución en el entorno si tu organización requiere que los agentes y flujos de trabajo se creen en una solución.

> [!NOTE]
> Compilar y probar un agente con tecnología del arnés de GitHub Copilot podría consumir Copilot Credits.

## Ejercicio 1: Crear el agente

### Tarea 1.1: Crear y configurar el agente de alertas de tareas de Contoso

1. Ve a [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) en `https://copilotstudio.microsoft.com/` e inicia sesión.

1. Confirma que esté seleccionado el entorno correcto.

1. En la página **Home**, selecciona **Agent**. Como alternativa, selecciona **Agents** en el panel de navegación izquierdo y, después, selecciona **New agent**.

1. En el campo **Name**, escribe `Agente de alertas de tareas de Contoso <tus iniciales>`. Reemplaza `<tus iniciales>` por tus iniciales u otro valor único.

1. Si tu organización requiere una solución, en la esquina superior derecha, selecciona **More options** (**...**) y, después, selecciona **Settings**. En **Agent settings** > **Agent details**, revisa la lista **Solution**. Si la solución proporcionada para el ejercicio aún no está seleccionada, selecciónala y, después, selecciona **Done**. Si ya está seleccionada, no necesitas seleccionar **Done**; cierra el cuadro de diálogo de configuración.

1. En **Instructions**, escribe el siguiente texto:

   ```prompt
   Eres el Agente de alertas de tareas de Contoso. Ayudas a los miembros del equipo de operaciones a enviar correos electrónicos de notificación de tareas.

   ## Tu función

   - Ayuda a los usuarios a enviar correos electrónicos de notificación de tareas con contenido coherente y preciso.
   - Responde preguntas sobre el proceso de notificación.
   - No envíes una notificación a menos que el usuario te lo solicite explícitamente.

   ## Cuando un usuario solicite enviar una notificación de tarea

   1. Identifica los siguientes detalles obligatorios en la conversación:
      - Dirección de correo electrónico profesional o educativa completa
      - Título de la tarea
      - Detalles o descripción de la tarea
      - Fecha de vencimiento

   2. Si falta algún detalle obligatorio, pídeselo al usuario antes de continuar.

   3. Resume los detalles de la notificación para el usuario y pídele que confirme antes de enviarla.

   4. Cuando el usuario confirme, usa la herramienta Enviar correo de notificación de tarea para enviar la notificación.

   5. Después de que se ejecute el flujo de trabajo, comunica la confirmación al usuario.

   ## Límites

   - No adivines ni infieras la dirección de correo electrónico del destinatario. Confírmala siempre con el usuario.
   - No envíes notificaciones a varios destinatarios en una sola llamada al flujo de trabajo.
   - Si el usuario cancela en cualquier momento, confirma la cancelación y no envíes el correo electrónico.
   ```

1. Selecciona **Save**.

## Ejercicio 2: Crear el flujo de trabajo

### Tarea 2.1: Crear el flujo de trabajo para enviar correos de notificación de tareas

1. En el panel de navegación izquierdo, selecciona **Workflows**.

1. Selecciona **New workflow**.

1. Selecciona el desencadenador **Start** en el lienzo.

1. En el panel derecho, establece **Trigger type** en **When an agent calls the workflow**. Confirma que el lienzo contenga tanto el desencadenador **When an agent calls the workflow** como la acción conectada **Respond to the agent**. Si falta la acción de respuesta, agrega **Respond to the agent** después del desencadenador.

### Tarea 2.2: Cambiar el nombre del flujo de trabajo

1. Selecciona **Workflow name** en la parte superior izquierda del lienzo del diseñador.

1. En **Workflow name**, escribe:

   ```text
   Enviar correo de notificación de tarea <tus iniciales>
   ```

   Reemplaza `<tus iniciales>` por un identificador único, como tus iniciales. Por ejemplo: `Enviar correo de notificación de tarea CS`.

1. Selecciona **Save**.

1. Continúa trabajando en el lienzo del flujo de trabajo.

### Tarea 2.3: Configurar las entradas del desencadenador

1. Selecciona el desencadenador **When an agent calls the flow** para abrir su panel de configuración en el lado derecho del lienzo del diseñador.

1. Selecciona **+ Add an input** y configura la primera entrada:

   | Campo | Valor |
   | --- | --- |
   | Type | **Text** |
   | Input name | `Correo electrónico del destinatario` |
   | Description | `La dirección de correo electrónico profesional o educativa completa de la persona que debe recibir la notificación de tarea. No adivines ni infieras este valor.` |

1. Selecciona **+ Add an input** y configura la segunda entrada:

   | Campo | Valor |
   | --- | --- |
   | Type | **Text** |
   | Input name | `Título de la tarea` |
   | Description | `El título breve de la tarea a la que se refiere la notificación.` |

1. Selecciona **+ Add an input** y configura la tercera entrada:

   | Campo | Valor |
   | --- | --- |
   | Type | **Text** |
   | Input name | `Detalles de la tarea` |
   | Description | `Una breve descripción de lo que implica la tarea, incluido cualquier contexto relevante.` |

1. Selecciona **+ Add an input** y configura la cuarta entrada:

   | Campo | Valor |
   | --- | --- |
   | Type | **Text** |
   | Input name | `Fecha de vencimiento` |
   | Description | `La fecha de vencimiento de la tarea, expresada tal como la proporcionó el usuario.` |

1. Selecciona **Save** para guardar tu progreso.

### Tarea 2.4: Agregar la acción Send an email (V2)

1. Selecciona el icono **+** entre el desencadenador **When an agent calls the flow** y la acción **Respond to the agent** para insertar una nueva acción.

1. En el campo de búsqueda, escribe `Send an email`.

1. En los resultados, selecciona la acción **Send an email** (también puede aparecer como **Send an email (V2)**) del conector **Office 365 Outlook**.

   Si es la primera vez que usas este conector, Copilot Studio puede pedirte que inicies sesión. Inicia sesión con la cuenta de Microsoft 365 que tiene el buzón de Outlook que quieres usar para enviar notificaciones.

   > [!NOTE]
   > Si se bloquea una ventana emergente del navegador, permite las ventanas emergentes de `https://copilotstudio.microsoft.com` y, después, vuelve a seleccionar la opción de inicio de sesión.

1. En el campo **To**, selecciona el icono **Insert dynamic content** y selecciona **Correo electrónico del destinatario**.

1. En el campo **Subject**, escribe lo siguiente e inserta contenido dinámico donde se indique:

   ```text
   Notificación de tarea: [Título de la tarea]
   ```

   Para redactarlo:

   - Escribe `Notificación de tarea:` seguido de un espacio como texto estático.
   - En el campo **Subject**, selecciona el icono **Insert dynamic content** (rayo) para abrir el panel de contenido dinámico.
   - En **When an agent calls the flow**, selecciona **Título de la tarea**.

1. En el campo **Body**, redacta el siguiente cuerpo del correo electrónico. Usa texto estático para las etiquetas y referencias de contenido dinámico para los valores:

   ```text
   Notificación de tarea

   Tarea: [Título de la tarea]
   Detalles: [Detalles de la tarea]
   Fecha de vencimiento: [Fecha de vencimiento]

   Esta notificación fue enviada por el Agente de alertas de tareas de Contoso.
   ```

   Para redactarlo:

   - Escribe `Notificación de tarea` en la primera línea y, después, agrega una línea en blanco.
   - Escribe `Tarea:` seguido de un espacio e inserta el contenido dinámico **Título de la tarea**.
   - Escribe `Detalles:` seguido de un espacio e inserta el contenido dinámico **Detalles de la tarea**.
   - Escribe `Fecha de vencimiento:` seguido de un espacio e inserta el contenido dinámico **Fecha de vencimiento**.
   - Agrega una línea en blanco y, después, escribe el texto de cierre como texto estático.

1. Deja todos los demás campos con sus valores predeterminados.

1. Selecciona **Save**.

### Tarea 2.5: Configurar las salidas de respuesta

1. Selecciona la acción **Respond to the agent** para abrir el panel de configuración.

1. Selecciona **+ Add an output** y configura la primera salida:

   | Campo | Valor |
   | --- | --- |
   | Type | **Text** |
   | Output name | `Estado` |
   | Output value | Escribe el texto estático: `Correo enviado` |

1. Selecciona **+ Add an output** y configura la segunda salida:

   | Campo | Valor |
   | --- | --- |
   | Type | **Text** |
   | Output name | `Confirmación` |
   | Output value | Combina `Notificación de tarea enviada a:` seguido de un espacio con la referencia de contenido dinámico **Correo electrónico del destinatario** de las entradas del desencadenador. |

1. En el panel de configuración **Respond to the agent**, selecciona **More options (...)** y, después, selecciona **Settings**.

1. Expande **Networking** y confirma que **Asynchronous response** esté establecido en **Off**. Si está activado, desactívalo para que el flujo de trabajo espere las salidas de respuesta y las devuelva al agente en tiempo real.

1. Selecciona **Save**.

### Tarea 2.6: Publicar el flujo de trabajo

1. Selecciona **Publish** en la esquina superior derecha del diseñador de flujos de trabajo.

1. Espera a que finalice el proceso de publicación.

1. Vuelve a la lista **Workflows** seleccionando **Workflows** en el panel de navegación izquierdo.

1. Comprueba que el flujo de trabajo que creaste muestre el estado **Published**.

   Si el estado muestra **Error**, selecciona el flujo de trabajo y revisa los detalles del error antes de continuar.

## Ejercicio 3: Agregar y configurar la herramienta de flujo de trabajo

### Tarea 3.1: Agregar el flujo de trabajo como una herramienta

1. Selecciona **Agents** en el panel de navegación izquierdo y abre el **Agente de alertas de tareas de Contoso** que creaste en el Ejercicio 1.

1. En la pestaña **Build**, selecciona la sección **Tools** del panel de componentes para abrir el cuadro de diálogo **Add a tool**.

1. En el cuadro de diálogo **Add a tool**, selecciona el filtro **Workflows**.

1. Selecciona el flujo de trabajo **Enviar correo de notificación de tarea** que publicaste. Confirma que el nombre coincida con el que creaste.

1. En la sección **Tools**, selecciona el flujo de trabajo para abrir su panel de configuración.

### Tarea 3.2: Configurar el nombre y la descripción de la herramienta

1. En el campo **Name**, comprueba o actualiza el nombre a:

   ```text
   Enviar correo de notificación de tarea <tus iniciales>
   ```

1. En el campo **Description for AI**, escribe:

   ```text
   Envía un correo electrónico con formato de notificación de tarea a un destinatario específico. Úsala cuando el usuario solicite enviar una notificación sobre una tarea y todos los detalles obligatorios estén disponibles y confirmados: correo electrónico del destinatario, título de la tarea, detalles de la tarea y fecha de vencimiento.
   ```

### Tarea 3.3: Configurar las entradas

En la sección **Inputs**, configura cada entrada:

1. En **Correo electrónico del destinatario**, confirma que la descripción diga: `La dirección de correo electrónico profesional o educativa completa de la persona que debe recibir la notificación de tarea. No adivines ni infieras este valor.`

1. En **Título de la tarea**, confirma que la descripción diga: `El título breve de la tarea a la que se refiere la notificación.`

1. En **Detalles de la tarea**, confirma que la descripción diga: `Una breve descripción de lo que implica la tarea, incluido cualquier contexto relevante.`

1. En **Fecha de vencimiento**, confirma que la descripción diga: `La fecha de vencimiento de la tarea, expresada tal como la proporcionó el usuario.`

### Tarea 3.4: Configurar las salidas del flujo de trabajo

1. En la pestaña **Outputs**, revisa las salidas del flujo de trabajo.

1. Comprueba que el flujo de trabajo incluya los parámetros de salida **Estado** y **Confirmación**.

   El agente usa las salidas `Estado` y `Confirmación` para redactar una respuesta de confirmación natural.

### Tarea 3.5: Guardar la configuración

1. Selecciona **Save** para aplicar la configuración de la herramienta.

1. Asegúrate de estar en la pestaña **Build** para actualizar las instrucciones del agente.

### Tarea 3.6: Hacer referencia a la herramienta en las instrucciones del agente

1. En la sección **Instructions**, busca la línea que dice:

   ```text
   4. Cuando el usuario confirme, usa la herramienta Enviar correo de notificación de tarea para enviar la notificación.
   ```

1. Coloca el cursor sobre `Enviar correo de notificación de tarea` y reemplázalo por el nombre exacto de la herramienta conectada: **Enviar correo de notificación de tarea**.

1. Selecciona **Save**.

## Ejercicio 4: Probar el agente y el flujo de trabajo

Abre la pestaña **Preview** en Copilot Studio. Usa las siguientes tareas para ejecutar las cuatro rutas de prueba.

### Tarea 4.1: Probar la ruta de éxito

1. En la parte superior del panel **Preview**, inicia una nueva sesión de conversación.

1. Escribe el siguiente mensaje. Reemplaza `<tu dirección de correo electrónico>` por tu propia dirección de correo electrónico profesional o educativa y `<fecha de vencimiento>` por una fecha que elijas del próximo mes natural.

   ```prompt
   Envía una notificación de tarea a <tu dirección de correo electrónico> para la tarea "Actualización de parches del servidor". La tarea consiste en aplicar los parches de seguridad mensuales a los servidores de producción. Vence el <fecha de vencimiento>.
   ```

   **Comportamiento esperado**:

   - El agente extrae del mensaje el correo electrónico del destinatario, el título de la tarea, los detalles de la tarea y la fecha de vencimiento.
   - El agente presenta la solicitud de confirmación que configuraste.

1. Aprueba la confirmación cuando se te solicite.

   **Comportamiento esperado después de la aprobación**:

   - El agente llama al flujo de trabajo.
   - El flujo de trabajo ejecuta la acción **Send an email**.
   - El agente responde con una confirmación que usa las salidas `Estado` y `Confirmación`.

1. Comprueba que la ejecución se haya realizado correctamente revisando la bandeja de entrada de la cuenta de correo electrónico que especificaste como destinatario. Confirma lo siguiente:

   - La línea de asunto dice `Notificación de tarea: Actualización de parches del servidor`.
   - El cuerpo contiene el título de la tarea, los detalles de la tarea y la fecha de vencimiento.
   - La línea de cierre dice `Esta notificación fue enviada por el Agente de alertas de tareas de Contoso.`

**Criterios de finalización de la Tarea 4.1**: El correo electrónico llega con el asunto, el cuerpo y el formato correctos, y el agente confirma la acción en la conversación.

### Tarea 4.2: Probar la ruta de información faltante

1. Inicia una nueva sesión de conversación en la pestaña **Preview**.

1. Escribe el siguiente mensaje:

   ```text
   Envía una notificación para la tarea "Validación de copias de seguridad".
   ```

   **Comportamiento esperado**:

   - El agente reconoce que falta información obligatoria.
   - El agente solicita las entradas que faltan, incluidas la dirección de correo electrónico del destinatario y la fecha de vencimiento.

1. Proporciona cada dato faltante a medida que el agente lo solicite. Usa estos valores:

   - **Título de la tarea**: `Validación de copias de seguridad.`
   - **Correo electrónico del destinatario**: Tu propia dirección de correo electrónico profesional o educativa.
   - **Detalles de la tarea**: `Comprueba que las copias de seguridad nocturnas se hayan completado correctamente y revisa los registros de las copias de seguridad.`
   - **Fecha de vencimiento**: Elige una fecha del próximo mes natural.

1. Cuando el agente presente la solicitud de confirmación, apruébala.

   **Comportamiento esperado después de la aprobación**:

   - El flujo de trabajo se ejecuta y se envía el correo electrónico.
   - El agente confirma la acción.

**Criterios de finalización de la Tarea 4.2**: El agente solicita las entradas faltantes en lugar de adivinarlas u omitirlas, y el flujo de trabajo se ejecuta correctamente después de que se proporcione toda la información obligatoria.

### Tarea 4.3: Probar la cancelación en la solicitud de confirmación

1. Inicia una nueva sesión de conversación en la pestaña **Preview**.

1. Escribe el siguiente mensaje. Reemplaza `<tu dirección de correo electrónico>` por tu propia dirección de correo electrónico profesional o educativa y `<fecha de vencimiento>` por una fecha que elijas del próximo mes natural.

   ```text
   Envía una notificación de tarea a <tu dirección de correo electrónico> para "Revisión de acceso". La tarea consiste en revisar y eliminar las cuentas de usuario inactivas. Vence el <fecha de vencimiento>.
   ```

1. Cuando el agente presente la solicitud de confirmación, recházala escribiendo:

   ```text
   Cancela, no envíes el correo electrónico.
   ```

   **Comportamiento esperado**:

   - El agente confirma la cancelación.
   - No se envía ningún correo electrónico.
   - El agente permanece listo para el siguiente mensaje de la misma conversación.

1. Comprueba que no llegue ningún correo electrónico. Abre la pestaña **Activity** del flujo de trabajo y confirma que la cancelación no haya creado una ejecución del flujo de trabajo.

**Criterios de finalización de la Tarea 4.3**: El agente cancela correctamente, no envía el correo electrónico y sigue respondiendo en la conversación.

### Tarea 4.4: Probar una ruta de error controlado

Esta prueba ejecuta el flujo de trabajo directamente con una dirección de correo electrónico mal formada de manera intencional. Después, inspeccionarás el panel **Activity** para comprender el error.

> [!NOTE]
> Esta prueba produce un error de manera intencional. El propósito es practicar el uso de la actividad del flujo de trabajo para diagnosticar problemas. No se envía ningún correo electrónico real a una dirección real.

1. En el panel de navegación izquierdo, selecciona **Workflows** y, después, selecciona el flujo de trabajo **Enviar correo de notificación de tarea** para abrirlo en el diseñador de flujos de trabajo. En la parte superior de la página, selecciona **Run** (**▶**) en la barra de comandos.

1. Escribe estos valores de prueba:

   | Entrada | Valor |
   | --- | --- |
   | Correo electrónico del destinatario | `not-an-email` |
   | Título de la tarea | `Informe de incidente` |
   | Detalles de la tarea | `Documenta la causa raíz de una interrupción reciente del servicio.` |
   | Fecha de vencimiento | Elige una fecha del próximo mes natural. |

1. Selecciona **Run**. El flujo de trabajo encuentra un error en el paso **Send an email (V2)** porque `not-an-email` no es una dirección válida.

1. Después del intento fallido, inspecciona la actividad del flujo de trabajo:

   1. En el diseñador de flujos de trabajo, abre la pestaña **Activity**.
   1. Filtra las ejecuciones por **Failed**.
   1. Selecciona la ejecución fallida más reciente para cargar los detalles de sus pasos en el lienzo.
   1. Identifica el paso que produjo el error (la acción **Send an email**) y lee el mensaje de error.

   **Qué debes buscar**:

   - La entrada **Correo electrónico del destinatario** recibió `not-an-email`, lo que confirma que la prueba del flujo de trabajo usó el valor que escribiste.
   - El paso **Send an email** muestra un error de validación o entrega porque la dirección está mal formada.
   - Todos los pasos anteriores, incluido el desencadenador, se ejecutaron sin errores.

1. Anota el tipo de error y el paso que produjo el error. Esta es la información que usarás para decidir si debes agregar validación de entradas, actualizar la descripción de la entrada o ajustar la solicitud de confirmación.

1. Restablece el funcionamiento normal regresando a la pestaña **Preview** del agente, iniciando una nueva conversación y completando otra vez la prueba de la ruta de éxito con tu propia dirección de correo electrónico.

**Criterios de finalización de la Tarea 4.4**: Puedes localizar la ejecución fallida en el panel **Activity**, identificar el paso que produjo el error y leer los detalles del error.

## Ejercicio 5: Comprobar y reflexionar

### Tarea 5.1: Revisar la configuración completa

Después de completar las cuatro rutas de prueba, revisa la configuración en su conjunto:

1. Abre la pestaña **Build** del agente y selecciona la herramienta **Enviar correo de notificación de tarea**.
1. Confirma que la descripción de la herramienta refleje con precisión lo que hace el flujo de trabajo y cuándo debe usarse.
1. Confirma que **Fill with AI** esté seleccionado para las cuatro entradas.
1. Confirma que la respuesta que devuelve el flujo de trabajo sea clara e informativa.

### Tarea 5.2: Opcional: Refinar según los resultados de las pruebas

Si alguna ruta de prueba reveló un problema, como que el agente adivinara el correo electrónico del destinatario en lugar de solicitarlo o que presentara una solicitud de confirmación poco clara, realiza un cambio específico y vuelve a probar la ruta afectada.

Mejoras comunes:

- **El agente adivinó el correo electrónico del destinatario**: Actualiza la descripción de la entrada **Correo electrónico del destinatario** para incluir: `No adivines ni infieras este valor; pregúntaselo al usuario si no lo proporciona.`
- **La solicitud de confirmación no fue clara**: Actualiza la solicitud para que muestre los valores específicos que usa el flujo de trabajo, como la dirección del destinatario y el título de la tarea.
- **El agente no seleccionó la herramienta**: Revisa la descripción de la herramienta y las instrucciones del agente. Es posible que la descripción sea demasiado imprecisa o que no coincida con el lenguaje que probablemente usen los usuarios.

## Limpieza

Después de completar el ejercicio, elimina los recursos que creaste para mantener ordenado el entorno.

1. **Quitar el flujo de trabajo del agente**:

   - Abre el agente, selecciona la pestaña **Build**, selecciona **Tools**, busca la herramienta **Enviar correo de notificación de tarea** y quítala.

1. **Eliminar el flujo de trabajo**:

   - En el panel de navegación izquierdo, selecciona **Workflows**.
   - Selecciona los puntos suspensivos (**...**) junto al flujo de trabajo **Enviar correo de notificación de tarea** que creaste.
   - Selecciona **Delete** y confirma la eliminación.

1. **Eliminar el agente**:

   - En el panel de navegación izquierdo, selecciona **Agents**.
   - Busca el **Agente de alertas de tareas de Contoso** que creaste, selecciona los puntos suspensivos (**...**) junto a él y selecciona **Delete**.
   - Confirma la eliminación.

> [!NOTE]
> Eliminar el flujo de trabajo antes de quitarlo del agente puede mostrar un error. Primero quita el flujo de trabajo del agente y, después, elimina el flujo de trabajo.
