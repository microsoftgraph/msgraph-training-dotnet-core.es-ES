<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="dbe3d-101">En este ejercicio, extenderá la aplicación desde el ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="dbe3d-102">Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="dbe3d-103">En este paso, integrará la Biblioteca de autenticación de [Microsoft (MSAL) para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="dbe3d-104">Inicialice el almacén secreto de desarrollo de [.NET](/aspnet/core/security/app-secrets) abriendo la CLI en el directorio que contiene **GraphTutorial.csproj** y ejecutando el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="dbe3d-105">Agregue el identificador de aplicación y una lista de ámbitos necesarios al almacén secreto mediante los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="dbe3d-106">Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación que creó en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    <span data-ttu-id="dbe3d-107">Veamos los ámbitos de permisos que acaba de establecer.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-107">Let's look at the permission scopes you just set.</span></span>

    - <span data-ttu-id="dbe3d-108">**User.Read** permitirá a la aplicación leer el perfil del usuario que ha iniciado sesión para obtener información como el nombre para mostrar y la dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-108">**User.Read** will allow the app to read the signed-in user's profile to get information such as display name and email address.</span></span>
    - <span data-ttu-id="dbe3d-109">**MailboxSettings.Read permitirá** a la aplicación leer la zona horaria preferida del usuario, el formato de fecha y el formato de hora.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-109">**MailboxSettings.Read** will allow the app to read the user's preferred time zone, date format, and time format.</span></span>
    - <span data-ttu-id="dbe3d-110">**Calendars.ReadWrite** permitirá a la aplicación leer los eventos existentes en el calendario del usuario y agregar nuevos eventos.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-110">**Calendars.ReadWrite** will allow the app to read the existing events on the user's calendar and add new events.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="dbe3d-111">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="dbe3d-111">Implement sign-in</span></span>

<span data-ttu-id="dbe3d-112">En esta sección, usarás la clase para solicitar un token de acceso `DeviceCodeCredential` mediante el flujo de código del [dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span><span class="sxs-lookup"><span data-stu-id="dbe3d-112">In this section you will use the `DeviceCodeCredential` class to request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

1. <span data-ttu-id="dbe3d-113">Cree un nuevo directorio en el **directorio GraphTutorial** denominado **Graph**.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-113">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="dbe3d-114">Cree un nuevo archivo en el directorio **Graph** denominado **GraphHelper.cs** y agregue el siguiente código a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-114">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Azure.Core;
    using Azure.Identity;
    using Microsoft.Graph;
    using TimeZoneConverter;

    namespace GraphTutorial
    {
        public class GraphHelper
        {
            private static DeviceCodeCredential tokenCredential;
            private static GraphServiceClient graphClient;

            public static void Initialize(string clientId,
                                          string[] scopes,
                                          Func<DeviceCodeInfo, CancellationToken, Task> callBack)
            {
                tokenCredential = new DeviceCodeCredential(callBack, clientId);
                graphClient = new GraphServiceClient(tokenCredential, scopes);
            }

            public static async Task<string> GetAccessTokenAsync(string[] scopes)
            {
                var context = new TokenRequestContext(scopes);
                var response = await tokenCredential.GetTokenAsync(context);
                return response.Token;
            }
        }
    }
    ```

1. <span data-ttu-id="dbe3d-115">Agregue la siguiente función a la clase `Program`.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-115">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="dbe3d-116">Agregue el siguiente código a la `Main` función inmediatamente después de la `Console.WriteLine(".NET Core Graph Tutorial\n");` línea.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-116">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="dbe3d-117">Agregue el siguiente código a la `Main` función inmediatamente después de la `// Display access token` línea.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-117">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="dbe3d-118">Compila y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-118">Build and run the app.</span></span> <span data-ttu-id="dbe3d-119">La aplicación muestra una dirección URL y un código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-119">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="dbe3d-120">Si encuentra errores, compare **Program.cs** con el [ejemplo de GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="dbe3d-120">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="dbe3d-121">Abra un explorador y vaya a la dirección URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-121">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="dbe3d-122">Escriba el código proporcionado e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-122">Enter the provided code and sign in.</span></span> <span data-ttu-id="dbe3d-123">Una vez completado, vuelva a la aplicación y elija **el 1. Muestra la opción de token** de acceso para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="dbe3d-123">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
