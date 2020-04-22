<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a6390-101">En este ejercicio, creará una nueva aplicación de Azure AD con el centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a6390-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="a6390-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com) e inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="a6390-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="a6390-103">Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones** en **administrar**.</span><span class="sxs-lookup"><span data-stu-id="a6390-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="a6390-104">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="a6390-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="a6390-105">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="a6390-105">Select **New registration**.</span></span> <span data-ttu-id="a6390-106">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="a6390-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="a6390-107">Establezca **Nombre** como `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="a6390-107">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="a6390-108">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="a6390-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="a6390-109">En **URI de redireccionamiento**, cambie la lista desplegable a **cliente público (móvil & escritorio)** y establezca `https://login.microsoftonline.com/common/oauth2/nativeclient`el valor en.</span><span class="sxs-lookup"><span data-stu-id="a6390-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. <span data-ttu-id="a6390-111">Seleccione **registrar**.</span><span class="sxs-lookup"><span data-stu-id="a6390-111">Select **Register**.</span></span> <span data-ttu-id="a6390-112">En la página **tutorial de .net Core Graph** , copie el valor del **identificador de la aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="a6390-112">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. <span data-ttu-id="a6390-114">Seleccione **Autenticación** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="a6390-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="a6390-115">Busque la sección **Configuración avanzada** y cambie el valor de la opción **tratar la aplicación como un cliente público** a **sí**y, a continuación, elija **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="a6390-115">Locate the **Advanced settings** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Captura de pantalla de la sección tipo de cliente predeterminado](./images/aad-default-client-type.png)
