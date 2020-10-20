<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c1590-101">En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="c1590-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="c1590-102">Abra **./Graph/GraphHelper.CS** y agregue la siguiente función a la clase **GraphHelper** .</span><span class="sxs-lookup"><span data-stu-id="c1590-102">Open **./Graph/GraphHelper.cs** and add the following function to the **GraphHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    <span data-ttu-id="c1590-103">Este código inicializa un objeto **Event** y usa el SDK de Graph para agregarlo al calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="c1590-103">This code initializes an **Event** object and uses the Graph SDK to add it to the user's calendar.</span></span>

1. <span data-ttu-id="c1590-104">Abra **./Program.CS** y agregue las siguientes funciones a la clase **Program** .</span><span class="sxs-lookup"><span data-stu-id="c1590-104">Open **./Program.cs** and add the following functions to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    <span data-ttu-id="c1590-105">Estas funciones son funciones auxiliares para leer la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="c1590-105">These functions are helper functions for reading user input.</span></span>

1. <span data-ttu-id="c1590-106">Agregue la siguiente función a la clase **Program** .</span><span class="sxs-lookup"><span data-stu-id="c1590-106">Add the following function to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    <span data-ttu-id="c1590-107">Esta función solicita al usuario el asunto, los asistentes, el inicio, el final y el cuerpo y, a continuación, usa esos valores para llamar `GraphHelper.CreateEvent` .</span><span class="sxs-lookup"><span data-stu-id="c1590-107">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `GraphHelper.CreateEvent`.</span></span>

1. <span data-ttu-id="c1590-108">Agregue lo siguiente justo después del `// Create a new event` Comentario en la `Main` función.</span><span class="sxs-lookup"><span data-stu-id="c1590-108">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. <span data-ttu-id="c1590-109">Guarde todos los cambios y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c1590-109">Save all of your changes and run the app.</span></span> <span data-ttu-id="c1590-110">Seleccione la opción **Agregar un evento** .</span><span class="sxs-lookup"><span data-stu-id="c1590-110">Choose the **Add an event** option.</span></span> <span data-ttu-id="c1590-111">Responda a los mensajes para crear un evento nuevo en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="c1590-111">Respond to the prompts to create a new event on the user's calendar.</span></span>
