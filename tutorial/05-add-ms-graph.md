<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="dd0fd-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="dd0fd-102">Para esta aplicación, usará la [biblioteca cliente .net de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="dd0fd-103">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="dd0fd-103">Get user details</span></span>

1. <span data-ttu-id="dd0fd-104">Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Graph**.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-104">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="dd0fd-105">Cree un nuevo archivo en el directorio de **Graph** denominado **GraphHelper.CS** y agregue el siguiente código a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-105">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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

1. <span data-ttu-id="dd0fd-106">Agregue el siguiente código en `Main` in **./Program.CS** justo después de la `GetAccessToken` llamada para obtener el usuario y generar el nombre para mostrar del usuario.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-106">Add the following code in `Main` in **./Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="dd0fd-107">Si ejecuta la aplicación ahora, una vez que inicie sesión en la aplicación, le agradecerá por su nombre.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-107">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="dd0fd-108">Obtener una vista de calendario</span><span class="sxs-lookup"><span data-stu-id="dd0fd-108">Get a calendar view</span></span>

1. <span data-ttu-id="dd0fd-109">Agregue la siguiente función a la `GraphHelper` clase para obtener los eventos del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-109">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="dd0fd-110">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="dd0fd-111">La dirección URL a la que se llamará es `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="dd0fd-111">The URL that will be called is `/me/calendarview`.</span></span>
- <span data-ttu-id="dd0fd-112">Los `startDateTime` `endDateTime` parámetros y definen el inicio y el final de la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-112">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
- <span data-ttu-id="dd0fd-113">El `Prefer: outlook.timezone` encabezado hace que el `start` y `end` de los eventos se devuelvan en la zona horaria del usuario.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-113">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
- <span data-ttu-id="dd0fd-114">La `Top` función solicita un máximo de 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-114">The `Top` function requests at most 50 events.</span></span>
- <span data-ttu-id="dd0fd-115">La `Select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-115">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="dd0fd-116">La `OrderBy` función ordena los resultados por fecha y hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-116">The `OrderBy` function sorts the results by the start date and time.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="dd0fd-117">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="dd0fd-117">Display the results</span></span>

1. <span data-ttu-id="dd0fd-118">Agregue la siguiente función a la `Program` clase para dar formato a las propiedades de [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph a un formato fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-118">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="dd0fd-119">Agregue la siguiente función a la `Program` clase para obtener los eventos del usuario y enviarlos a la consola.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-119">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="dd0fd-120">Agregue lo siguiente justo después del `// List the calendar` Comentario en la `Main` función.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-120">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. <span data-ttu-id="dd0fd-121">Guarde todos los cambios y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-121">Save all of your changes and run the app.</span></span> <span data-ttu-id="dd0fd-122">Elija la opción **Ver calendario de esta semana** para ver una lista de los eventos del usuario.</span><span class="sxs-lookup"><span data-stu-id="dd0fd-122">Choose the **View this week's calendar** option to see a list of the user's events.</span></span>

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
