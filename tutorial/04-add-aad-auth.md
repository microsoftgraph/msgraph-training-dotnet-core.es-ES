<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f8953-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8953-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="f8953-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f8953-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="f8953-103">En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f8953-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="f8953-104">Inicialice el [almacén secreto de desarrollo .net](/aspnet/core/security/app-secrets) abriendo la CLI en el directorio que contiene **GraphTutorial. csproj** y ejecutando el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="f8953-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="f8953-105">Agregue el identificador de la aplicación y una lista de ámbitos necesarios al almacén secreto mediante los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="f8953-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="f8953-106">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="f8953-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    <span data-ttu-id="f8953-107">Echemos un vistazo a los ámbitos de permisos que acaba de establecer.</span><span class="sxs-lookup"><span data-stu-id="f8953-107">Let's look at the permission scopes you just set.</span></span>

    - <span data-ttu-id="f8953-108">**User. Read** permitirá a la aplicación leer el perfil del usuario que ha iniciado sesión para obtener información, como el nombre para mostrar y la dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f8953-108">**User.Read** will allow the app to read the signed-in user's profile to get information such as display name and email address.</span></span>
    - <span data-ttu-id="f8953-109">**MailboxSettings. Read** permitirá que la aplicación Lea la zona horaria preferida del usuario, el formato de fecha y el formato de hora.</span><span class="sxs-lookup"><span data-stu-id="f8953-109">**MailboxSettings.Read** will allow the app to read the user's preferred time zone, date format, and time format.</span></span>
    - <span data-ttu-id="f8953-110">**Calendars. ReadWrite** permitirá que la aplicación Lea los eventos existentes en el calendario del usuario y agregue nuevos eventos.</span><span class="sxs-lookup"><span data-stu-id="f8953-110">**Calendars.ReadWrite** will allow the app to read the existing events on the user's calendar and add new events.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="f8953-111">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="f8953-111">Implement sign-in</span></span>

<span data-ttu-id="f8953-112">En esta sección, creará un proveedor de autenticación que se puede usar con el SDK de Graph y también se puede usar para solicitar explícitamente un token de acceso mediante el [flujo de código de dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span><span class="sxs-lookup"><span data-stu-id="f8953-112">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="f8953-113">Crear un proveedor de autenticación</span><span class="sxs-lookup"><span data-stu-id="f8953-113">Create an authentication provider</span></span>

1. <span data-ttu-id="f8953-114">Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Authentication (autenticación**).</span><span class="sxs-lookup"><span data-stu-id="f8953-114">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="f8953-115">Cree un nuevo archivo en el directorio de **autenticación** denominado **DeviceCodeAuthProvider.CS** y agregue el siguiente código a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="f8953-115">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Authentication/DeviceCodeAuthProvider.cs" id="AuthProviderSnippet":::

<span data-ttu-id="f8953-116">Considere lo que hace este código.</span><span class="sxs-lookup"><span data-stu-id="f8953-116">Consider what this code does.</span></span>

- <span data-ttu-id="f8953-117">Usa la implementación de MSAL `IPublicClientApplication` para solicitar y administrar los tokens.</span><span class="sxs-lookup"><span data-stu-id="f8953-117">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="f8953-118">La `GetAccessToken` función:</span><span class="sxs-lookup"><span data-stu-id="f8953-118">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="f8953-119">Inicia sesión en el usuario si aún no ha iniciado sesión con el flujo de código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="f8953-119">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="f8953-120">Garantiza que el token devuelto siempre se actualiza usando la `AcquireTokenSilent` función, que devuelve el token almacenado en caché si no ha expirado y actualiza el token si ha expirado.</span><span class="sxs-lookup"><span data-stu-id="f8953-120">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="f8953-121">Implementa la interfaz para `IAuthenticationProvider` que el SDK de Graph pueda usar la clase para autenticar las llamadas de gráficos.</span><span class="sxs-lookup"><span data-stu-id="f8953-121">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="f8953-122">Iniciar sesión y mostrar el token de acceso</span><span class="sxs-lookup"><span data-stu-id="f8953-122">Sign in and display the access token</span></span>

<span data-ttu-id="f8953-123">En esta sección se actualizará la aplicación para que llame a la `GetAccessToken` función, que iniciará sesión en el usuario.</span><span class="sxs-lookup"><span data-stu-id="f8953-123">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="f8953-124">También se agregará código para mostrar el token.</span><span class="sxs-lookup"><span data-stu-id="f8953-124">You will also add code to display the token.</span></span>

1. <span data-ttu-id="f8953-125">Agregue la siguiente función a la clase `Program`.</span><span class="sxs-lookup"><span data-stu-id="f8953-125">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="f8953-126">Agregue el siguiente código a la `Main` función inmediatamente después de la `Console.WriteLine(".NET Core Graph Tutorial\n");` línea.</span><span class="sxs-lookup"><span data-stu-id="f8953-126">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="f8953-127">Agregue el siguiente código a la `Main` función inmediatamente después de la `// Display access token` línea.</span><span class="sxs-lookup"><span data-stu-id="f8953-127">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="f8953-128">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f8953-128">Build and run the app.</span></span> <span data-ttu-id="f8953-129">La aplicación muestra una dirección URL y un código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="f8953-129">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="f8953-130">Si encuentra errores, compare su **Program.CS** con el [ejemplo en github](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="f8953-130">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="f8953-131">Abra un explorador y vaya a la dirección URL que se muestra.</span><span class="sxs-lookup"><span data-stu-id="f8953-131">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="f8953-132">Escriba el código proporcionado e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="f8953-132">Enter the provided code and sign in.</span></span> <span data-ttu-id="f8953-133">Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="f8953-133">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
