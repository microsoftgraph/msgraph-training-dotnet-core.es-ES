<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b119e-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b119e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="b119e-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b119e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="b119e-103">En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b119e-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

<span data-ttu-id="b119e-104">Cree un archivo nuevo en el directorio **GraphTutorial** denominado **appSettings. JSON**.</span><span class="sxs-lookup"><span data-stu-id="b119e-104">Create a new file in the **GraphTutorial** directory named **appsettings.json**.</span></span> <span data-ttu-id="b119e-105">Agregue el texto siguiente en el archivo.</span><span class="sxs-lookup"><span data-stu-id="b119e-105">Add the following text in that file.</span></span>

```json
{
  "appId": "YOUR_APP_ID_HERE",
  "scopes": [
    "User.Read",
    "Calendars.Read"
  ]
}
```

<span data-ttu-id="b119e-106">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="b119e-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b119e-107">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **appSettings. JSON** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b119e-107">If you're using source control such as git, now would be a good time to exclude the **appsettings.json** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="b119e-108">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="b119e-108">Implement sign-in</span></span>

<span data-ttu-id="b119e-109">En esta sección, creará un proveedor de autenticación que se puede usar con el SDK de Graph y también se puede usar para solicitar explícitamente un token de acceso mediante el [flujo de código de dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span><span class="sxs-lookup"><span data-stu-id="b119e-109">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="b119e-110">Crear un proveedor de autenticación</span><span class="sxs-lookup"><span data-stu-id="b119e-110">Create an authentication provider</span></span>

1. <span data-ttu-id="b119e-111">Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Authentication (autenticación**).</span><span class="sxs-lookup"><span data-stu-id="b119e-111">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="b119e-112">Cree un nuevo archivo en el directorio de **autenticación** denominado **DeviceCodeAuthProvider.CS** y agregue el siguiente código a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="b119e-112">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Identity.Client;
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace GraphTutorial
    {
        public class DeviceCodeAuthProvider : IAuthenticationProvider
        {
            private IPublicClientApplication _msalClient;
            private string[] _scopes;
            private IAccount _userAccount;

            public DeviceCodeAuthProvider(string appId, string[] scopes)
            {
                _scopes = scopes;

                _msalClient = PublicClientApplicationBuilder
                    .Create(appId)
                    // Set the tenant ID to "organizations" to disable personal accounts
                    // Azure OAuth does not support device code flow for personal accounts
                    // See https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code
                    .WithTenantId("organizations")
                    .Build();
            }

            public async Task<string> GetAccessToken()
            {
                // If there is no saved user account, the user must sign-in
                if (_userAccount == null)
                {
                    try
                    {
                        // Invoke device code flow so user can sign-in with a browser
                        var result = await _msalClient.AcquireTokenWithDeviceCode(_scopes, callback => {
                            Console.WriteLine(callback.Message);
                            return Task.FromResult(0);
                        }).ExecuteAsync();

                        _userAccount = result.Account;
                        return result.AccessToken;
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"Error getting access token: {exception.Message}");
                        return null;
                    }
                }
                else
                {
                    // If there is an account, call AcquireTokenSilent
                    // By doing this, MSAL will refresh the token automatically if
                    // it is expired. Otherwise it returns the cached token.

                        var result = await _msalClient
                            .AcquireTokenSilent(_scopes, _userAccount)
                            .ExecuteAsync();

                       return result.AccessToken;
                }
            }

            // This is the required function to implement IAuthenticationProvider
            // The Graph SDK will call this function each time it makes a Graph
            // call.
            public async Task AuthenticateRequestAsync(HttpRequestMessage requestMessage)
            {
                requestMessage.Headers.Authorization =
                    new AuthenticationHeaderValue("bearer", await GetAccessToken());
            }
        }
    }
    ```

<span data-ttu-id="b119e-113">Considere lo que hace este código.</span><span class="sxs-lookup"><span data-stu-id="b119e-113">Consider what this code does.</span></span>

- <span data-ttu-id="b119e-114">Usa la implementación de `IPublicClientApplication` MSAL para solicitar y administrar los tokens.</span><span class="sxs-lookup"><span data-stu-id="b119e-114">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="b119e-115">La `GetAccessToken` función:</span><span class="sxs-lookup"><span data-stu-id="b119e-115">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="b119e-116">Inicia sesión en el usuario si aún no ha iniciado sesión con el flujo de código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="b119e-116">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="b119e-117">Garantiza que el token devuelto siempre se actualiza usando la `AcquireTokenSilent` función, que devuelve el token almacenado en caché si no ha expirado y actualiza el token si ha expirado.</span><span class="sxs-lookup"><span data-stu-id="b119e-117">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="b119e-118">Implementa la `IAuthenticationProvider` interfaz para que el SDK de Graph pueda usar la clase para autenticar las llamadas de gráficos.</span><span class="sxs-lookup"><span data-stu-id="b119e-118">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="b119e-119">Iniciar sesión y mostrar el token de acceso</span><span class="sxs-lookup"><span data-stu-id="b119e-119">Sign in and display the access token</span></span>

<span data-ttu-id="b119e-120">En esta sección se actualizará la aplicación para que llame `GetAccessToken` a la función, que iniciará sesión en el usuario.</span><span class="sxs-lookup"><span data-stu-id="b119e-120">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="b119e-121">También se agregará código para mostrar el token.</span><span class="sxs-lookup"><span data-stu-id="b119e-121">You will also add code to display the token.</span></span>

1. <span data-ttu-id="b119e-122">Abra **Program.CS** y agregue las siguientes `using` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="b119e-122">Open **Program.cs** and add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="b119e-123">Agregue la siguiente función a la clase `Program`.</span><span class="sxs-lookup"><span data-stu-id="b119e-123">Add the following function to the `Program` class.</span></span>

    ```csharp
    static IConfigurationRoot LoadAppSettings()
    {
        var appConfig = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json", false, true)
            .Build();

        // Check for required settings
        if (string.IsNullOrEmpty(appConfig["appId"]) ||
            // Make sure there's at least one value in the scopes array
            string.IsNullOrEmpty(appConfig["scopes:0"]))
        {
            return null;
        }

        return appConfig;
    }
    ```

1. <span data-ttu-id="b119e-124">Agregue el siguiente código a la `Main` función inmediatamente después de `Console.WriteLine(".NET Core Graph Tutorial\n");` la línea.</span><span class="sxs-lookup"><span data-stu-id="b119e-124">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    ```csharp
    var appConfig = LoadAppSettings();

    if (appConfig == null)
    {
        Console.WriteLine("Missing or invalid appsettings.json...exiting");
        return;
    }

    var appId = appConfig["appId"];
    var scopes = appConfig.GetSection("scopes").Get<string[]>();

    // Initialize the auth provider with values from appsettings.json
    var authProvider = new DeviceCodeAuthProvider(appId, scopes);

    // Request a token to sign in the user
    var accessToken = authProvider.GetAccessToken().Result;
    ```

1. <span data-ttu-id="b119e-125">Agregue el siguiente código a la `Main` función inmediatamente después de `// Display access token` la línea.</span><span class="sxs-lookup"><span data-stu-id="b119e-125">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="b119e-126">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b119e-126">Build and run the app.</span></span> <span data-ttu-id="b119e-127">La aplicación muestra una dirección URL y un código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="b119e-127">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="b119e-128">Si encuentra errores, compare su **Program.CS** con el [ejemplo en github](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="b119e-128">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="b119e-129">Abra un explorador y vaya a la dirección URL que se muestra.</span><span class="sxs-lookup"><span data-stu-id="b119e-129">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="b119e-130">Escriba el código proporcionado e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="b119e-130">Enter the provided code and sign in.</span></span> <span data-ttu-id="b119e-131">Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="b119e-131">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
