<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a5b38-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a5b38-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="a5b38-102">Para esta aplicación, usará la [biblioteca cliente .net de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a5b38-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="a5b38-103">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="a5b38-103">Get user details</span></span>

1. <span data-ttu-id="a5b38-104">Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Graph**.</span><span class="sxs-lookup"><span data-stu-id="a5b38-104">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="a5b38-105">Cree un nuevo archivo en el directorio de **Graph** denominado **GraphHelper.CS** y agregue el siguiente código a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="a5b38-105">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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
                    return await graphClient.Me.Request().GetAsync();
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

1. <span data-ttu-id="a5b38-106">Agregue el siguiente código en `Main` **Program.CS** justo después de la `GetAccessToken` llamada para obtener el usuario y generar el nombre para mostrar del usuario.</span><span class="sxs-lookup"><span data-stu-id="a5b38-106">Add the following code in `Main` in **Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="a5b38-107">Si ejecuta la aplicación ahora, una vez que inicie sesión en la aplicación, le agradecerá por su nombre.</span><span class="sxs-lookup"><span data-stu-id="a5b38-107">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="a5b38-108">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="a5b38-108">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="a5b38-109">Agregue la siguiente función a la `GraphHelper` clase para obtener los eventos del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="a5b38-109">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="a5b38-110">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="a5b38-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="a5b38-111">La dirección URL a la que se `/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="a5b38-111">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="a5b38-112">La `Select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.</span><span class="sxs-lookup"><span data-stu-id="a5b38-112">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="a5b38-113">La `OrderBy` función ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="a5b38-113">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="a5b38-114">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="a5b38-114">Display the results</span></span>

1. <span data-ttu-id="a5b38-115">Agregue la siguiente función a la `Program` clase para dar formato a las propiedades de [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph a un formato fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="a5b38-115">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="a5b38-116">Agregue la siguiente función a la `Program` clase para obtener los eventos del usuario y enviarlos a la consola.</span><span class="sxs-lookup"><span data-stu-id="a5b38-116">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="a5b38-117">Agregue lo siguiente justo después del `// List the calendar` comentario en la `Main` función.</span><span class="sxs-lookup"><span data-stu-id="a5b38-117">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents();
    ```

1. <span data-ttu-id="a5b38-118">Guarde todos los cambios y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a5b38-118">Save all of your changes and run the app.</span></span> <span data-ttu-id="a5b38-119">Elija la opción **lista de eventos de calendario** para ver una lista de los eventos del usuario.</span><span class="sxs-lookup"><span data-stu-id="a5b38-119">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM
      End: 5/22/19, 4:00 PM
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM
      End: 5/24/19, 8:00 PM
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM
      End: 5/26/19, 7:00 PM
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM
      End: 5/27/19, 10:30 PM
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM
      End: 5/28/19, 5:00 PM
    ```
