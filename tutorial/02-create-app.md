<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="87f10-101">Para empezar, cree un nuevo proyecto de consola principal de .NET con [.net Core CLI](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="87f10-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="87f10-102">Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="87f10-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="87f10-103">Ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="87f10-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="87f10-104">Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="87f10-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="87f10-105">Si funciona, la aplicación debe obtener resultados `Hello World!` .</span><span class="sxs-lookup"><span data-stu-id="87f10-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="87f10-106">Instalar dependencias</span><span class="sxs-lookup"><span data-stu-id="87f10-106">Install dependencies</span></span>

<span data-ttu-id="87f10-107">Antes de continuar, agregue más dependencias adicionales que usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="87f10-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="87f10-108">[Microsoft.Extensions.Configuration. UserSecrets](https://github.com/aspnet/extensions) para leer la configuración de la aplicación desde el [almacén secreto de desarrollo .net](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="87f10-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="87f10-109">[Biblioteca de autenticación de Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) para autenticar al usuario y adquirir tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="87f10-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="87f10-110">[Biblioteca cliente .net de Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="87f10-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="87f10-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para traducir los identificadores de zona horaria de Windows a identificadores de IANA.</span><span class="sxs-lookup"><span data-stu-id="87f10-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

<span data-ttu-id="87f10-112">Ejecute los siguientes comandos en su CLI para instalar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="87f10-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.8
dotnet add package Microsoft.Identity.Client --version 4.19.0
dotnet add package Microsoft.Graph --version 3.15.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a><span data-ttu-id="87f10-113">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="87f10-113">Design the app</span></span>

<span data-ttu-id="87f10-114">En esta sección, creará un menú sencillo basado en consola.</span><span class="sxs-lookup"><span data-stu-id="87f10-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="87f10-115">Abra **./Program.CS** en un editor de texto (como [Visual Studio Code](https://code.visualstudio.com/)) y reemplace todo su contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="87f10-115">Open **./Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

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

<span data-ttu-id="87f10-116">Esto implementa un menú básico y lee la elección del usuario de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="87f10-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
