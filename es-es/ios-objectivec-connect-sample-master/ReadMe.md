# <a name="office-365-connect-sample-for-ios-using-the-microsoft-graph-sdk"></a>Ejemplo Connect de Office 365 para iOS con SDK de Microsoft Graph

Microsoft Graph es un punto de conexión unificado para acceder a los datos, las relaciones y datos que proceden de Microsoft Cloud. Este ejemplo muestra cómo conectarse, autenticarse y, después, llamar a la API de usuario y correo a través del [SDK de Microsoft Graph para iOS](https://github.com/microsoftgraph/msgraph-sdk-ios).

> Nota: Consulte la página del [Portal de registro de la aplicación de Microsoft Graph](https://apps.dev.microsoft.com) que simplifica el registro para poder conseguir que este ejemplo se ejecute más rápidamente.

## <a name="prerequisites"></a>Requisitos previos
* Descargue [Xcode](https://developer.apple.com/xcode/downloads/) de Apple.

* Instale [CocoaPods](https://guides.cocoapods.org/using/using-cocoapods.html) como administrador de dependencias.
* Una cuenta de correo electrónico personal o profesional de Microsoft como Office 365, outlook.com, hotmail.com, etc. Puede registrarse para [una suscripción de Office 365 Developer](https://aka.ms/devprogramsignup), que incluye los recursos que necesita para comenzar a crear aplicaciones de Office 365.

     > Nota: Si ya dispone de una suscripción, el vínculo anterior le dirige a una página con el mensaje *No se puede agregar a su cuenta actual*. En ese caso, use una cuenta de su suscripción actual a Office 365.    
* Un Id. de cliente de la aplicación registrada en el [Portal de registro de la aplicación de Microsoft Graph](https://apps.dev.microsoft.com)
* Para realizar solicitudes, se debe proporcionar un **MSAuthenticationProvider** que sea capaz de autenticar solicitudes HTTPS con un token de portador OAuth 2.0 adecuado. Usaremos [msgraph-sdk-ios-nxoauth2-adapter](https://github.com/microsoftgraph/msgraph-sdk-ios-nxoauth2-adapter) para una implementación de MSAuthenticationProvider que puede usarse para poner en marcha el proyecto. Consulte la sección **Código de interés** para obtener más información.


## <a name="running-this-sample-in-xcode"></a>Ejecutar este ejemplo en Xcode

1. Clonar este repositorio
2. Si no está instalado ya, ejecute los comandos siguientes de la aplicación **Terminal** para instalar y configurar el administrador de dependencias CocoaPods.

        sudo gem install cocoapods
    
        pod setup

2. Use CocoaPods para importar el SDK de Microsoft Graph y las dependencias de autenticación:

        pod 'MSGraphSDK'
        pod 'MSGraphSDK-NXOAuth2Adapter'


 Esta aplicación de ejemplo ya contiene un podfile que recibirá los pods en el proyecto. Solo tiene que ir a la raíz del proyecto donde esté el podfile y ejecutar desde **Terminal**:

        pod install

   Para obtener más información, consulte **Usar CocoaPods** en [Recursos adicionales](#AdditionalResources)

3. Abra **O365-iOS-Microsoft-Graph-SDK.xcworkspace**
4. Abra **AuthenticationConstants.m**. Verá que el **ClientID** del proceso de registro se puede agregar a la parte superior del archivo:

        // You will set your application's clientId
        NSString * const kClientId    = @"ENTER_YOUR_CLIENT_ID";

    > Nota: Observará que se han configurado los siguientes ámbitos de permiso para este proyecto: **"https://graph.microsoft.com/Mail.Send", "https://graph.microsoft.com/User.Read", "offline_access"**. Las llamadas de servicio usadas en este proyecto, el envío de un correo a su cuenta de correo y la recuperación de la información de perfil (nombre para mostrar, dirección de correo electrónico) requieren estos permisos para que la aplicación se ejecute correctamente.

5. Ejecute el ejemplo. Deberá conectarse a una cuenta de correo personal o profesional, o autenticarlas, y, después, puede enviar un correo a esa cuenta, o a otra cuenta de correo electrónico seleccionada.


##<a name="code-of-interest"></a>Código de interés

Todos los códigos de autenticación que se pueden ver en el archivo **AuthenticationProvider.m**. Usamos una implementación de ejemplo de MSAuthenticationProvider de [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) para proporcionar compatibilidad de inicio de sesión a aplicaciones nativas registradas, actualización automática de los tokens de acceso y la característica de cierre de sesión:

        [[NXOAuth2AuthenticationProvider sharedAuthProvider] loginWithViewController:nil completion:^(NSError *error) {
            if (!error) {
            [MSGraphClient setAuthenticationProvider:[NXOAuth2AuthenticationProvider sharedAuthProvider]];
            self.client = [MSGraphClient client];
             }
        }];


Una vez se defina el proveedor de autenticación, podemos crear e inicializar un objeto de cliente (MSGraphClient) que se usará para realizar llamadas en el punto de conexión de servicio de Microsoft Graph (correo y usuarios). En **SendMailViewcontroller.m** podemos ensamblar una solicitud de correo y enviarla mediante el código siguiente:

    MSGraphUserSendMailRequestBuilder *requestBuilder = [[self.client me]sendMailWithMessage:message saveToSentItems:true];    
    MSGraphUserSendMailRequest *mailRequest = [requestBuilder request];   
    [mailRequest executeWithCompletion:^(NSDictionary *response, NSError *error) {      
    }];


Para obtener más información, incluido el código para llamar a otros servicios, como OneDrive, consulte el [GDK de Microsoft Graph para iOS](https://github.com/microsoftgraph/msgraph-sdk-ios)

## <a name="questions-and-comments"></a>Preguntas y comentarios

Nos encantaría recibir sus comentarios sobre el proyecto Connect de Office 365 para iOS con Microsoft Graph. Puede enviarnos sus preguntas y sugerencias a través de la sección [Problemas](https://github.com/microsoftgraph/iOS-objectivec-connect-sample/issues) de este repositorio.

Las preguntas generales sobre desarrollo en Office 365 deben publicarse en [Stack Overflow](http://stackoverflow.com/questions/tagged/Office365+API). Asegúrese de que sus preguntas o comentarios se etiquetan con [Office365] y [MicrosoftGraph].

## <a name="contributing"></a>Colaboradores
Deberá firmar un [Contrato de licencia de colaborador](https://cla.microsoft.com/) antes de enviar la solicitud de incorporación de cambios. Para completar el Contrato de licencia de colaborador (CLA), deberá enviar una solicitud mediante el formulario y, después, firmar electrónicamente el CLA cuando reciba el correo electrónico que contiene el vínculo al documento.

Este proyecto ha adoptado el [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Código de conducta de código abierto de Microsoft). Para obtener más información, consulte las [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) (Preguntas más frecuentes del código de conducta) o póngase en contacto con [opencode@microsoft.com](mailto:opencode@microsoft.com) con otras preguntas o comentarios.

## <a name="additional-resources"></a>Recursos adicionales

* [Centro de desarrollo de Office](http://dev.office.com/)
* [Página de información general de Microsoft Graph](https://graph.microsoft.io)
* [Usar CocoaPods](https://guides.cocoapods.org/using/using-cocoapods.html)

## <a name="copyright"></a>Copyright
Copyright (c) 2016 Microsoft. Todos los derechos reservados.
