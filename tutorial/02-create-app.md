<!-- markdownlint-disable MD002 MD041 -->

Comience creando un nuevo proyecto de consola de .NET Core con [la CLI de .NET Core](/dotnet/core/tools/).

1. Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto. Ejecuta el siguiente comando.

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.

    ```Shell
    dotnet run
    ```

    Si funciona, la aplicación debe generar `Hello World!` .

## <a name="install-dependencies"></a>Instalar dependencias

Antes de seguir adelante, agrega algunas dependencias adicionales que usarás más adelante.

- [Microsoft.Extensions.Configuration. UserSecrets](https://github.com/aspnet/extensions) para leer la configuración de la aplicación desde el almacén secreto [de desarrollo de .NET](https://docs.microsoft.com/aspnet/core/security/app-secrets).
- [Biblioteca de cliente del SDK de Azure para Azure Identity para](https://github.com/Azure/azure-sdk-for-net) autenticar al usuario y adquirir tokens de acceso.
- [Microsoft Graph biblioteca de cliente de .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a microsoft Graph.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para traducir Windows de zona horaria a identificadores de IANA.

Ejecute los siguientes comandos en la CLI para instalar las dependencias.

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Azure.Identity --version 1.4.0
dotnet add package Microsoft.Graph --version 4.0.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, creará un menú sencillo basado en consola.

Abra **./Program.cs** en un editor de texto (como [Visual Studio Code](https://code.visualstudio.com/)) y reemplace todo su contenido por el código siguiente.

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.Extensions.Configuration;

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
