<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la [biblioteca cliente .net de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.

## <a name="get-user-details"></a>Obtener detalles del usuario

1. Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Graph**.
1. Cree un nuevo archivo en el directorio de **Graph** denominado **GraphHelper.CS** y agregue el siguiente código a ese archivo.

    ```csharp
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;

    namespace GraphTutorial
    {
        public class GraphHelper
        {
            private static GraphServiceClient graphClient;
            public static void Initialize(IAuthenticationProvider authProvider)
            {
                graphClient = new GraphServiceClient(authProvider);
            }

            public static async Task<User> GetMeAsync()
            {
                try
                {
                    // GET /me
                    return await graphClient.Me
                        .Request()
                        .Select(u => new{
                            u.DisplayName,
                            u.MailboxSettings
                        })
                        .GetAsync();
                }
                catch (ServiceException ex)
                {
                    Console.WriteLine($"Error getting signed-in user: {ex.Message}");
                    return null;
                }
            }
        }
    }
    ```

1. Agregue el siguiente código en `Main` in **./Program.CS** justo después de la `GetAccessToken` llamada para obtener el usuario y generar el nombre para mostrar del usuario.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

Si ejecuta la aplicación ahora, una vez que inicie sesión en la aplicación, le agradecerá por su nombre.

## <a name="get-a-calendar-view"></a>Obtener una vista de calendario

1. Agregue la siguiente función a la `GraphHelper` clase para obtener los eventos del calendario del usuario.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

Tenga en cuenta lo que está haciendo este código.

- La dirección URL a la que se llamará es `/me/calendarview` .
- Los `startDateTime` `endDateTime` parámetros y definen el inicio y el final de la vista de calendario.
- El `Prefer: outlook.timezone` encabezado hace que el `start` y `end` de los eventos se devuelvan en la zona horaria del usuario.
- La `Top` función solicita un máximo de 50 eventos.
- La `Select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.
- La `OrderBy` función ordena los resultados por fecha y hora de inicio.

## <a name="display-the-results"></a>Mostrar los resultados

1. Agregue la siguiente función a la `Program` clase para dar formato a las propiedades de [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph a un formato fácil de uso.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. Agregue la siguiente función a la `Program` clase para obtener los eventos del usuario y enviarlos a la consola.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. Agregue lo siguiente justo después del `// List the calendar` Comentario en la `Main` función.

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. Guarde todos los cambios y ejecute la aplicación. Elija la opción **Ver calendario de esta semana** para ver una lista de los eventos del usuario.

    ```Shell
    Welcome Lynne Robbins!

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 10:00 AM
      End: 9/28/2020 11:30 AM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 2:00 PM
      End: 9/28/2020 3:00 PM
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 9/28/2020 4:00 PM
      End: 9/28/2020 5:30 PM
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 9/29/2020 12:00 PM
      End: 9/29/2020 1:00 PM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/29/2020 2:00 PM
      End: 9/29/2020 3:00 PM
    Subject: Project Tailspin
    ```
