<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, extenderá la aplicación desde el ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la Biblioteca de autenticación de [Microsoft (MSAL) para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) en la aplicación.

1. Inicialice el almacén secreto de desarrollo de [.NET](/aspnet/core/security/app-secrets) abriendo la CLI en el directorio que contiene **GraphTutorial.csproj** y ejecutando el siguiente comando.

    ```Shell
    dotnet user-secrets init
    ```

1. Agregue el identificador de aplicación y una lista de ámbitos necesarios al almacén secreto mediante los siguientes comandos. Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación que creó en Azure Portal.

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    Veamos los ámbitos de permisos que acaba de establecer.

    - **User.Read** permitirá a la aplicación leer el perfil del usuario que ha iniciado sesión para obtener información como el nombre para mostrar y la dirección de correo electrónico.
    - **MailboxSettings.Read permitirá** a la aplicación leer la zona horaria preferida del usuario, el formato de fecha y el formato de hora.
    - **Calendars.ReadWrite** permitirá a la aplicación leer los eventos existentes en el calendario del usuario y agregar nuevos eventos.

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

En esta sección, usarás la clase para solicitar un token de acceso `DeviceCodeCredential` mediante el flujo de código del [dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).

1. Cree un nuevo directorio en el **directorio GraphTutorial** denominado **Graph**.
1. Cree un nuevo archivo en el directorio **Graph** denominado **GraphHelper.cs** y agregue el siguiente código a ese archivo.

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

1. Agregue la siguiente función a la clase `Program`.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. Agregue el siguiente código a la `Main` función inmediatamente después de la `Console.WriteLine(".NET Core Graph Tutorial\n");` línea.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. Agregue el siguiente código a la `Main` función inmediatamente después de la `// Display access token` línea.

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. Compila y ejecuta la aplicación. La aplicación muestra una dirección URL y un código de dispositivo.

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > Si encuentra errores, compare **Program.cs** con el [ejemplo de GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).

1. Abra un explorador y vaya a la dirección URL mostrada. Escriba el código proporcionado e inicie sesión. Una vez completado, vuelva a la aplicación y elija **el 1. Muestra la opción de token** de acceso para mostrar el token de acceso.
