<!-- markdownlint-disable MD002 MD041 -->

Comience por crear un nuevo proyecto de consola de .NET Core con [la CLI de .NET Core.](/dotnet/core/tools/)

1. Abra la interfaz de línea de comandos (CLI) en un directorio donde quiera crear el proyecto. Ejecute el comando siguiente.

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.

    ```Shell
    dotnet run
    ```

    Si funciona, la aplicación debe generar `Hello World!` un resultado.

## <a name="install-dependencies"></a>Instalar dependencias

Antes de seguir adelante, agregue algunas dependencias adicionales que usará más adelante.

- [Microsoft.Extensions.Configurl. UserSecrets](https://github.com/aspnet/extensions) para leer la configuración de la aplicación desde el almacén secreto [de desarrollo .NET](https://docs.microsoft.com/aspnet/core/security/app-secrets).
- [Biblioteca de autenticación de Microsoft (MSAL) para que .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) autentique al usuario y adquiera tokens de acceso.
- [Biblioteca de cliente .NET de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para traducir identificadores de zona horaria de Windows a identificadores IANA.

Ejecute los siguientes comandos en la CLI para instalar las dependencias.

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Microsoft.Identity.Client --version 4.25.0
dotnet add package Microsoft.Graph --version 3.22.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección crearás un menú sencillo basado en consola.

Abra **./Program.cs** en un editor de texto (como [Visual Studio code)](https://code.visualstudio.com/)y reemplace todo su contenido por el código siguiente.

```csharp
using Microsoft.Extensions.Configuration;
using System;
using System.Collections.Generic;

namespace GraphTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(".NET Core Graph Tutorial\n");

            int choice = -1;

            while (choice != 0) {
                Console.WriteLine("Please choose one of the following options:");
                Console.WriteLine("0. Exit");
                Console.WriteLine("1. Display access token");
                Console.WriteLine("2. View this week's calendar");
                Console.WriteLine("3. Add an event");

                try
                {
                    choice = int.Parse(Console.ReadLine());
                }
                catch (System.FormatException)
                {
                    // Set to invalid value
                    choice = -1;
                }

                switch(choice)
                {
                    case 0:
                        // Exit the program
                        Console.WriteLine("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    case 3:
                        // Create a new event
                        break;
                    default:
                        Console.WriteLine("Invalid choice! Please try again.");
                        break;
                }
            }
        }
    }
}
```

Esto implementa un menú básico y lee la elección del usuario desde la línea de comandos.
