<!-- markdownlint-disable MD002 MD041 -->

Para empezar, cree un nuevo proyecto de consola principal de .NET con [.net Core CLI](/dotnet/core/tools/?tabs=netcore2x).

1. Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto. Ejecute el comando siguiente.

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.

    ```Shell
    dotnet run
    ```

    Si funciona, la aplicación debe obtener resultados `Hello World!`.

## <a name="install-dependencies"></a>Instalar dependencias

Antes de continuar, agregue más dependencias adicionales que usará más adelante.

- [Microsoft. Extensions. Configuration](https://github.com/aspnet/Extensions) para leer la configuración de la aplicación de un archivo JSON.
- [Biblioteca de autenticación de Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) para autenticar al usuario y adquirir tokens de acceso.
- [Biblioteca cliente .net de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.
- [Proveedores de autenticación para el SDK de Microsoft Graph .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet-auth) para habilitar la biblioteca de cliente de Graph para que solicite tokens automáticamente al realizar llamadas a la API.

Ejecute los siguientes comandos en su CLI para instalar las dependencias.

```Shell
dotnet add package Microsoft.Extensions.Configuration --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.FileExtensions --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Json --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Binder --version 2.2.0
dotnet add package Microsoft.Identity.Client --version 4.3.1
dotnet add package Microsoft.Graph --version 1.17.0
```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, creará un menú sencillo basado en consola.

Abra **Program.CS** en un editor de texto (como [Visual Studio Code](https://code.visualstudio.com/)) y reemplace todo el contenido por el código siguiente.

[!code-csharp[](../demos/01-create-app/GraphTutorial/Program.cs)]

Esto implementa un menú básico y lee la elección del usuario de la línea de comandos.
