<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) en la aplicación.

1. Inicialice el [almacén secreto de desarrollo .net](/aspnet/core/security/app-secrets) abriendo la CLI en el directorio que contiene **GraphTutorial. csproj** y ejecutando el siguiente comando.

    ```Shell
    dotnet user-secrets init
    ```

1. Agregue el identificador de la aplicación y una lista de ámbitos necesarios al almacén secreto mediante los comandos siguientes. Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

En esta sección, creará un proveedor de autenticación que se puede usar con el SDK de Graph y también se puede usar para solicitar explícitamente un token de acceso mediante el [flujo de código de dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).

### <a name="create-an-authentication-provider"></a>Crear un proveedor de autenticación

1. Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Authentication (autenticación**).
1. Cree un nuevo archivo en el directorio de **autenticación** denominado **DeviceCodeAuthProvider.CS** y agregue el siguiente código a ese archivo.

    :::code language="csharp" source="../demo/GraphTutorial/Authentication/DeviceCodeAuthProvider.cs" id="AuthProviderSnippet":::

Considere lo que hace este código.

- Usa la implementación de `IPublicClientApplication` MSAL para solicitar y administrar los tokens.
- La `GetAccessToken` función:
  - Inicia sesión en el usuario si aún no ha iniciado sesión con el flujo de código de dispositivo.
  - Garantiza que el token devuelto siempre se actualiza usando la `AcquireTokenSilent` función, que devuelve el token almacenado en caché si no ha expirado y actualiza el token si ha expirado.
- Implementa la `IAuthenticationProvider` interfaz para que el SDK de Graph pueda usar la clase para autenticar las llamadas de gráficos.

## <a name="sign-in-and-display-the-access-token"></a>Iniciar sesión y mostrar el token de acceso

En esta sección se actualizará la aplicación para que llame `GetAccessToken` a la función, que iniciará sesión en el usuario. También se agregará código para mostrar el token.

1. Agregue la siguiente función a la clase `Program`.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. Agregue el siguiente código a la `Main` función inmediatamente después de `Console.WriteLine(".NET Core Graph Tutorial\n");` la línea.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. Agregue el siguiente código a la `Main` función inmediatamente después de `// Display access token` la línea.

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. Compile y ejecute la aplicación. La aplicación muestra una dirección URL y un código de dispositivo.

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > Si encuentra errores, compare su **Program.CS** con el [ejemplo en github](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).

1. Abra un explorador y vaya a la dirección URL que se muestra. Escriba el código proporcionado e inicie sesión. Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.
