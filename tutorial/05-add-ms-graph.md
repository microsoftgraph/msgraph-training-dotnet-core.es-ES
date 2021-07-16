<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2ddbf-101">En este ejercicio, incorporará el Graph Microsoft en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="2ddbf-102">Para esta aplicación, usará la biblioteca de [cliente de .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) de Microsoft Graph para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="2ddbf-103">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="2ddbf-103">Get user details</span></span>

1. <span data-ttu-id="2ddbf-104">Abra **./Graph/GraphHelper.cs** y agregue la siguiente función a la **clase GraphHelper.**</span><span class="sxs-lookup"><span data-stu-id="2ddbf-104">Open **./Graph/GraphHelper.cs** and add the following function to the **GraphHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetMeSnippet":::

1. <span data-ttu-id="2ddbf-105">Agregue el siguiente código en `Main` **./Program.cs** justo después de la llamada para obtener el usuario y generar el nombre `GetAccessToken` para mostrar del usuario.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-105">Add the following code in `Main` in **./Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="2ddbf-106">Si ejecutas la aplicación ahora, después de iniciar sesión en la aplicación te da la bienvenida por su nombre.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-106">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="2ddbf-107">Obtener una vista de calendario</span><span class="sxs-lookup"><span data-stu-id="2ddbf-107">Get a calendar view</span></span>

1. <span data-ttu-id="2ddbf-108">Agregue la siguiente función a la `GraphHelper` clase para obtener eventos del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-108">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="2ddbf-109">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-109">Consider what this code is doing.</span></span>

- <span data-ttu-id="2ddbf-110">La dirección URL a la que se llamará es `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-110">The URL that will be called is `/me/calendarview`.</span></span>
- <span data-ttu-id="2ddbf-111">Los `startDateTime` parámetros y definen el inicio y el final de la vista de `endDateTime` calendario.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-111">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
- <span data-ttu-id="2ddbf-112">El `Prefer: outlook.timezone` encabezado hace que el y de los eventos se `start` `end` devuelvan en la zona horaria del usuario.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-112">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
- <span data-ttu-id="2ddbf-113">La `Top` función solicita como máximo 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-113">The `Top` function requests at most 50 events.</span></span>
- <span data-ttu-id="2ddbf-114">La `Select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-114">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="2ddbf-115">La `OrderBy` función ordena los resultados por la fecha y hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-115">The `OrderBy` function sorts the results by the start date and time.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="2ddbf-116">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="2ddbf-116">Display the results</span></span>

1. <span data-ttu-id="2ddbf-117">Agregue la siguiente función a la clase para dar formato a las propiedades `Program` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph un formato fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-117">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="2ddbf-118">Agregue la siguiente función a la clase para obtener los eventos del usuario y `Program` generarlos en la consola.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-118">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="2ddbf-119">Agregue lo siguiente justo después del `// List the calendar` comentario en la `Main` función.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-119">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. <span data-ttu-id="2ddbf-120">Guarda todos los cambios y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-120">Save all of your changes and run the app.</span></span> <span data-ttu-id="2ddbf-121">Elija la **opción Ver calendario de esta** semana para ver una lista de los eventos del usuario.</span><span class="sxs-lookup"><span data-stu-id="2ddbf-121">Choose the **View this week's calendar** option to see a list of the user's events.</span></span>

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
