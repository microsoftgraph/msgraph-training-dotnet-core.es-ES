<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5dfab-101">En este ejercicio, creará una nueva aplicación de Azure AD mediante el Centro Azure Active Directory administración.</span><span class="sxs-lookup"><span data-stu-id="5dfab-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="5dfab-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com) e inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="5dfab-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="5dfab-103">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="5dfab-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="5dfab-104">Captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5dfab-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="5dfab-105">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="5dfab-105">Select **New registration**.</span></span> <span data-ttu-id="5dfab-106">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="5dfab-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="5dfab-107">Establezca **Nombre** como `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="5dfab-107">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="5dfab-108">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="5dfab-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="5dfab-109">En **URI de** redireccionamiento, cambie el desplegable a Cliente público **(escritorio & móvil)** y establezca el valor en `https://login.microsoftonline.com/common/oauth2/nativeclient` .</span><span class="sxs-lookup"><span data-stu-id="5dfab-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>

    ![Captura de pantalla de la página Registrar una aplicación](./images/aad-register-an-app.png)

1. <span data-ttu-id="5dfab-111">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="5dfab-111">Select **Register**.</span></span> <span data-ttu-id="5dfab-112">En la página tutorial de **.NET Core Graph,** copie el valor del identificador de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="5dfab-112">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Una captura de pantalla del Id. de aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. <span data-ttu-id="5dfab-114">Seleccione **Autenticación** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="5dfab-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="5dfab-115">Busque la **sección Configuración avanzada** y cambie la alternancia Permitir flujos de **cliente** público a **Sí** y, a continuación, **elija Guardar**.</span><span class="sxs-lookup"><span data-stu-id="5dfab-115">Locate the **Advanced settings** section and change the **Allow public client flows** toggle to **Yes**, then choose **Save**.</span></span>

    ![Captura de pantalla de la alternancia Permitir flujos de cliente público](./images/aad-default-client-type.png)
