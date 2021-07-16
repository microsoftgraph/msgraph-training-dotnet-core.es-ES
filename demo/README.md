# <a name="how-to-run-the-completed-project"></a>Cómo ejecutar el proyecto completado

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:

- SDK [de .NET Core](https://dotnet.microsoft.com/download) instalado en el equipo de desarrollo. (**Nota: este** tutorial se escribió con .NET Core SDK versión 3.1.402. Los pasos de esta guía pueden funcionar con otras versiones, pero eso no se ha probado).
- Una cuenta laboral o educativa de Microsoft.

Si no tiene una cuenta de [](https://developer.microsoft.com/office/dev-program) Microsoft, puede registrarse en el programa de desarrolladores de Office 365 para obtener una suscripción Office 365 gratuita.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar una aplicación web con el Centro Azure Active Directory administración

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com) e inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Captura de pantalla de los registros de la aplicación ](/tutorial/images/aad-portal-app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación**, establezca los valores siguientes.

    - Establezca **Nombre** como `.NET Core Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en Cuentas en cualquier directorio de la **organización.**
    - Deje **URI de redireccionamiento** vacía.

    ![Captura de pantalla de la página Registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. Seleccione **Registrar**. En la página tutorial de **.NET Core Graph,** copie el valor del identificador de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Una captura de pantalla del Id. de aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. Seleccione el **vínculo Agregar un URI de redireccionamiento.** En la **página Uri de redireccionamiento,** busque la sección URI de redireccionamiento sugerido para clientes **públicos (móvil, escritorio).** Seleccione el `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Captura de pantalla de la página Uri de redireccionamiento](/tutorial/images/aad-redirect-uris.png)

1. Busque la **sección Configuración avanzada** y cambie la alternancia Permitir flujos de **cliente** público a **Sí** y, a continuación, **elija Guardar**.

    ![Captura de pantalla de la sección Tipo de cliente predeterminado](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Configuración del ejemplo

1. Inicialice el almacén secreto de desarrollo de [.NET](https://docs.microsoft.com/aspnet/core/security/app-secrets) abriendo la CLI en el directorio que contiene **GraphTutorial.csproj** y ejecutando el siguiente comando.

    ```Shell
    dotnet user-secrets init
    ```

1. Agregue el identificador de aplicación y una lista de ámbitos necesarios al almacén secreto mediante los siguientes comandos. Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación que creó en Azure Portal.

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a>Compilar y ejecutar el ejemplo

En la interfaz de línea de comandos (CLI), vaya al directorio del proyecto y ejecute los siguientes comandos.

```Shell
dotnet restore
dotnet build
dotnet run
```
