<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio incorporará Microsoft Graph en la aplicación. Para esta aplicación, usará la biblioteca cliente [.NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) de Microsoft Graph para realizar llamadas a Microsoft Graph.

## <a name="get-user-details"></a>Obtener detalles del usuario

1. Cree un nuevo directorio en el **directorio GraphTutorial** denominado **Graph**.
1. Cree un nuevo archivo en el directorio **de Graph** denominado **GraphHelper.cs** y agregue el siguiente código a ese archivo.

    ```csharp
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using TimeZoneConverter;

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

1. Agregue el siguiente código en ./Program.cs justo después de la llamada para obtener el usuario y generar el nombre para mostrar `Main` del  `GetAccessToken` usuario.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

Si ejecutas la aplicación ahora, después de iniciar sesión en la aplicación te da la bienvenida por su nombre.

## <a name="get-a-calendar-view"></a>Obtener una vista de calendario

1. Agregue la siguiente función a la `GraphHelper` clase para obtener eventos del calendario del usuario.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

Ten en cuenta lo que hace este código.

- La dirección URL a la que se llamará es `/me/calendarview` .
- Los `startDateTime` parámetros y definen el inicio y el final de la vista de `endDateTime` calendario.
- El `Prefer: outlook.timezone` encabezado hace que los eventos y los eventos se `start` `end` devuelvan en la zona horaria del usuario.
- La `Top` función solicita como máximo 50 eventos.
- La `Select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.
- La `OrderBy` función ordena los resultados por fecha y hora de inicio.

## <a name="display-the-results"></a>Mostrar los resultados

1. Agregue la siguiente función a la clase para dar formato a las propiedades `Program` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph en un formato fácil de usar.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. Agregue la siguiente función a la clase para obtener los eventos del usuario y `Program` generarlos en la consola.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. Agregue lo siguiente justo después del `// List the calendar` comentario en la `Main` función.

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. Guarda todos los cambios y ejecuta la aplicación. Elija la **opción Ver calendario de esta** semana para ver una lista de los eventos del usuario.

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
