---
title: aaaSilent installera Azure AD App Proxy connector | Microsoft Docs
description: "Beskriver hur tooperform en obevakad installation av Azure AD Application Proxy Connector tooprovide säker fjärråtkomst tooyour lokala appar."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3aa1c7f2-fb2a-4693-abd5-95bb53700cbb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ce796ff45a65ba7d5f0f63c02085bdc6af494548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="silently-install-hello-azure-ad-application-proxy-connector"></a>Obevakad installation hello Azure AD Application Proxy Connector
Vill du toobe kan toosend en installation skript toomultiple Windows eller tooWindows servrar som inte har aktiverat användargränssnittet. Det här avsnittet hjälper dig att skapa en Windows PowerShell-skript som aktiverar obevakad installation och registrering för din Azure AD Application Proxy Connector.

Den här funktionen är användbar när du vill:

* Installera hello connector på datorer utan användargränssnitt skikt eller när det går inte att RDP toohello datorn.
* Installera och registrera många kopplingar på samma gång.
* Integrera hello connector installation och registrering som en del av en annan procedur.
* Skapa en standard serveravbildning som innehåller hello connector bitar men har inte registrerats.

Programproxy fungerar genom att installera en smidig Windows Server-tjänsten som kallas hello Connector i ditt nätverk. Hello Application Proxy Connector toowork har toobe registrerad med Azure AD-katalogen med en global administratör och lösenord. Den här informationen anges normalt under installationen av koppling i en dialogruta. Du kan dock använda Windows PowerShell toocreate en autentiseringsuppgift objektet tooenter registreringsinformationen. Du kan skapa egna token och använda den tooenter registreringsinformationen.

## <a name="install-hello-connector"></a>Installera hello connector
Installera hello Connector MSI: er utan att registrera hello Connector på följande sätt:

1. Öppna en kommandotolk.
2. Kör följande kommando i vilka hello /q innebär tyst installation hello - hello installationen uppmanas inte tooaccept hello licensavtal (EULA).
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-hello-connector-with-azure-ad"></a>Registrera hello connector med Azure AD
Det finns två metoder som du kan använda tooregister hello anslutningen:

* Registrera hello connector med hjälp av Windows PowerShell-autentiseringsobjekt
* Registrera hello connector med hjälp av en token som skapats offline

### <a name="register-hello-connector-using-a-windows-powershell-credential-object"></a>Registrera hello connector med hjälp av Windows PowerShell-autentiseringsobjekt
1. Skapa hello Windows PowerShell-autentiseringsuppgifter objekt genom att köra det här kommandot. Ersätt  *\<användarnamn\>*  och  *\<lösenord\>*  med hello användarnamn och lösenord för din katalog:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Gå för**C:\Program Files\Microsoft AAD App Proxy Connector** och köra hello skript med hello PowerShell autentiseringsuppgifter objekt du har skapat. Ersätt *$cred* med hello namnet hello PowerShell autentiseringsuppgifter objekt du har skapat:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-hello-connector-using-a-token-created-offline"></a>Registrera hello connector med hjälp av en token som skapats offline
1. Skapa en offline-token med hello värden i hello kodstycke hello AuthenticationContext-klassen:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// hello AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// hello application ID of hello connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// hello reply address of hello connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// hello AppIdUri of hello registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }


2. När du har hello token kan skapa en SecureString använder hello-token:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Kör hello följande Windows PowerShell-kommandot, ersätter \<klient GUID\> med katalog-ID:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Nästa steg 
* [Publicera program med ditt domännamn](active-directory-application-proxy-custom-domains.md)
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
* [Felsökning av problem med Application Proxy](active-directory-application-proxy-troubleshoot.md)


