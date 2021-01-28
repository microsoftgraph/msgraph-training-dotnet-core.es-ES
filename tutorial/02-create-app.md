<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="03f68-101">Comience por crear un nuevo proyecto de consola de .NET Core con [la CLI de .NET Core.](/dotnet/core/tools/)</span><span class="sxs-lookup"><span data-stu-id="03f68-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="03f68-102">Abra la interfaz de línea de comandos (CLI) en un directorio donde quiera crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="03f68-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="03f68-103">Ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="03f68-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="03f68-104">Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="03f68-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="03f68-105">Si funciona, la aplicación debe generar `Hello World!` un resultado.</span><span class="sxs-lookup"><span data-stu-id="03f68-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="03f68-106">Instalar dependencias</span><span class="sxs-lookup"><span data-stu-id="03f68-106">Install dependencies</span></span>

<span data-ttu-id="03f68-107">Antes de seguir adelante, agregue algunas dependencias adicionales que usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="03f68-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="03f68-108">[Microsoft.Extensions.Configurl. UserSecrets](https://github.com/aspnet/extensions) para leer la configuración de la aplicación desde el almacén secreto [de desarrollo .NET](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="03f68-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="03f68-109">[Biblioteca de autenticación de Microsoft (MSAL) para que .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) autentique al usuario y adquiera tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="03f68-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="03f68-110">[Biblioteca de cliente .NET de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="03f68-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="03f68-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para traducir identificadores de zona horaria de Windows a identificadores IANA.</span><span class="sxs-lookup"><span data-stu-id="03f68-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

<span data-ttu-id="03f68-112">Ejecute los siguientes comandos en la CLI para instalar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="03f68-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Microsoft.Identity.Client --version 4.25.0
dotnet add package Microsoft.Graph --version 3.22.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a><span data-ttu-id="03f68-113">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="03f68-113">Design the app</span></span>

<span data-ttu-id="03f68-114">En esta sección crearás un menú sencillo basado en consola.</span><span class="sxs-lookup"><span data-stu-id="03f68-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="03f68-115">Abra **./Program.cs** en un editor de texto (como [Visual Studio code)](https://code.visualstudio.com/)y reemplace todo su contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="03f68-115">Open **./Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

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

<span data-ttu-id="03f68-116">Esto implementa un menú básico y lee la elección del usuario desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="03f68-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
