<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

1. Abra **./Graph/GraphHelper.CS** y agregue la siguiente función a la clase **GraphHelper** .

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    Este código inicializa un objeto **Event** y usa el SDK de Graph para agregarlo al calendario del usuario.

1. Abra **./Program.CS** y agregue las siguientes funciones a la clase **Program** .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    Estas funciones son funciones auxiliares para leer la entrada del usuario.

1. Agregue la siguiente función a la clase **Program** .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    Esta función solicita al usuario el asunto, los asistentes, el inicio, el final y el cuerpo y, a continuación, usa esos valores para llamar `GraphHelper.CreateEvent` .

1. Agregue lo siguiente justo después del `// Create a new event` Comentario en la `Main` función.

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. Guarde todos los cambios y ejecute la aplicación. Seleccione la opción **Agregar un evento** . Responda a los mensajes para crear un evento nuevo en el calendario del usuario.
