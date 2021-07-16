# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="efe90-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="efe90-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efe90-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="efe90-102">Prerequisites</span></span>

<span data-ttu-id="efe90-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="efe90-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="efe90-104">SDK [de .NET Core](https://dotnet.microsoft.com/download) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="efe90-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="efe90-105">(**Nota: este** tutorial se escribió con .NET Core SDK versión 3.1.402.</span><span class="sxs-lookup"><span data-stu-id="efe90-105">(**Note:** This tutorial was written with .NET Core SDK version 3.1.402.</span></span> <span data-ttu-id="efe90-106">Los pasos de esta guía pueden funcionar con otras versiones, pero eso no se ha probado).</span><span class="sxs-lookup"><span data-stu-id="efe90-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="efe90-107">Una cuenta laboral o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="efe90-107">A Microsoft work or school account.</span></span>

<span data-ttu-id="efe90-108">Si no tiene una cuenta de [](https://developer.microsoft.com/office/dev-program) Microsoft, puede registrarse en el programa de desarrolladores de Office 365 para obtener una suscripción Office 365 gratuita.</span><span class="sxs-lookup"><span data-stu-id="efe90-108">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="efe90-109">Registrar una aplicación web con el Centro Azure Active Directory administración</span><span class="sxs-lookup"><span data-stu-id="efe90-109">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="efe90-110">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com) e inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="efe90-110">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="efe90-111">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="efe90-111">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="efe90-112">Captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="efe90-112">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="efe90-113">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="efe90-113">Select **New registration**.</span></span> <span data-ttu-id="efe90-114">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="efe90-114">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="efe90-115">Establezca **Nombre** como `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="efe90-115">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="efe90-116">Establezca **Tipos de cuenta admitidos** en Cuentas en cualquier directorio de la **organización.**</span><span class="sxs-lookup"><span data-stu-id="efe90-116">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="efe90-117">Deje **URI de redireccionamiento** vacía.</span><span class="sxs-lookup"><span data-stu-id="efe90-117">Leave **Redirect URI** empty.</span></span>

    ![Captura de pantalla de la página Registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="efe90-119">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="efe90-119">Select **Register**.</span></span> <span data-ttu-id="efe90-120">En la página tutorial de **.NET Core Graph,** copie el valor del identificador de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="efe90-120">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Una captura de pantalla del Id. de aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="efe90-122">Seleccione el **vínculo Agregar un URI de redireccionamiento.**</span><span class="sxs-lookup"><span data-stu-id="efe90-122">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="efe90-123">En la **página Uri de redireccionamiento,** busque la sección URI de redireccionamiento sugerido para clientes **públicos (móvil, escritorio).**</span><span class="sxs-lookup"><span data-stu-id="efe90-123">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="efe90-124">Seleccione el `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="efe90-124">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Captura de pantalla de la página Uri de redireccionamiento](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="efe90-126">Busque la **sección Configuración avanzada** y cambie la alternancia Permitir flujos de **cliente** público a **Sí** y, a continuación, **elija Guardar**.</span><span class="sxs-lookup"><span data-stu-id="efe90-126">Locate the **Advanced settings** section and change the **Allow public client flows** toggle to **Yes**, then choose **Save**.</span></span>

    ![Captura de pantalla de la sección Tipo de cliente predeterminado](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="efe90-128">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="efe90-128">Configure the sample</span></span>

1. <span data-ttu-id="efe90-129">Inicialice el almacén secreto de desarrollo de [.NET](https://docs.microsoft.com/aspnet/core/security/app-secrets) abriendo la CLI en el directorio que contiene **GraphTutorial.csproj** y ejecutando el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="efe90-129">Initialize the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="efe90-130">Agregue el identificador de aplicación y una lista de ámbitos necesarios al almacén secreto mediante los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="efe90-130">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="efe90-131">Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación que creó en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="efe90-131">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a><span data-ttu-id="efe90-132">Compilar y ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="efe90-132">Build and run the sample</span></span>

<span data-ttu-id="efe90-133">En la interfaz de línea de comandos (CLI), vaya al directorio del proyecto y ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="efe90-133">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
dotnet restore
dotnet build
dotnet run
```
