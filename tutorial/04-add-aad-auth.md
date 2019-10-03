<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) en la aplicación.

Cree un archivo nuevo en el directorio **GraphTutorial** denominado **appSettings. JSON**. Agregue el texto siguiente en el archivo.

```json
{
  "appId": "YOUR_APP_ID_HERE",
  "scopes": [
    "User.Read",
    "Calendars.Read"
  ]
}
```

Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.

> [!IMPORTANT]
> Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **appSettings. JSON** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

En esta sección, creará un proveedor de autenticación que se puede usar con el SDK de Graph y también se puede usar para solicitar explícitamente un token de acceso mediante el [flujo de código de dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).

### <a name="create-an-authentication-provider"></a>Crear un proveedor de autenticación

1. Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Authentication (autenticación**).
1. Cree un nuevo archivo en el directorio de **autenticación** denominado **DeviceCodeAuthProvider.CS** y agregue el siguiente código a ese archivo.

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

Considere lo que hace este código.

- Usa la implementación de `IPublicClientApplication` MSAL para solicitar y administrar los tokens.
- La `GetAccessToken` función:
  - Inicia sesión en el usuario si aún no ha iniciado sesión con el flujo de código de dispositivo.
  - Garantiza que el token devuelto siempre se actualiza usando la `AcquireTokenSilent` función, que devuelve el token almacenado en caché si no ha expirado y actualiza el token si ha expirado.
- Implementa la `IAuthenticationProvider` interfaz para que el SDK de Graph pueda usar la clase para autenticar las llamadas de gráficos.

## <a name="sign-in-and-display-the-access-token"></a>Iniciar sesión y mostrar el token de acceso

En esta sección se actualizará la aplicación para que llame `GetAccessToken` a la función, que iniciará sesión en el usuario. También se agregará código para mostrar el token.

1. Abra **Program.CS** y agregue las siguientes `using` instrucciones en la parte superior del archivo.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Agregue la siguiente función a la clase `Program`.

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

1. Agregue el siguiente código a la `Main` función inmediatamente después de `Console.WriteLine(".NET Core Graph Tutorial\n");` la línea.

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
    > Si encuentra errores, compare su **Program.CS** con el [ejemplo en github](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).

1. Abra un explorador y vaya a la dirección URL que se muestra. Escriba el código proporcionado e inicie sesión. Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.
