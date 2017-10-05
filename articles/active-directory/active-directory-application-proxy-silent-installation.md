---
title: Tyst installation Azure AD App Proxy connector | Microsoft Docs
description: "Beskriver hur du utför en obevakad installation av Azure AD Application Proxy Connector att tillhandahålla säker fjärråtkomst till lokala appar."
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
ms.openlocfilehash: 9e28c89d8f64f0ae3d4150017ca544e606075c45
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="silently-install-the-azure-ad-application-proxy-connector"></a>Installera Azure AD Application Proxy Connector
Du vill kunna skicka ett skript för installation på flera Windows-servrar eller på Windows-servrar som inte har aktiverat användargränssnittet. Det här avsnittet hjälper dig att skapa en Windows PowerShell-skript som aktiverar obevakad installation och registrering för din Azure AD Application Proxy Connector.

Den här funktionen är användbar när du vill:

* Installera connector på datorer utan användargränssnitt skikt eller när det går inte att RDP till datorn.
* Installera och registrera många kopplingar på samma gång.
* Integrera kopplingsinstallationen och registrering som en del av en annan procedur.
* Skapa en standard serveravbildning som innehåller connector bitar men har inte registrerats.

Programproxy fungerar genom att installera en smidig Windows Server-tjänsten som kallas kopplingen i nätverket. Application Proxy Connector fungerar den har i registreras med Azure AD-katalogen med en global administratör och lösenord. Den här informationen anges normalt under installationen av koppling i en dialogruta. Du kan dock använda Windows PowerShell för att skapa ett autentiseringsuppgiftobjekt för att ange din registreringsinformation. Eller skapa egna token och använda den för att ange din registreringsinformation.

## <a name="install-the-connector"></a>Installera connector
Installera Connector-MSI: er utan att registrera kopplingen på följande sätt:

1. Öppna en kommandotolk.
2. Kör följande kommando där /q innebär tyst installation - uppmanas installationen inte att godkänna licensavtalet.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Registrera anslutningsverktyget med Azure AD
Det finns två metoder som du kan använda för att registrera anslutningsverktyget:

* Registrera anslutningsverktyget med hjälp av Windows PowerShell-autentiseringsobjekt
* Registrera anslutningsverktyget med hjälp av en token som skapats offline

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrera anslutningsverktyget med hjälp av Windows PowerShell-autentiseringsobjekt
1. Skapa Windows PowerShell-autentiseringsuppgifter objektet genom att köra det här kommandot. Ersätt  *\<användarnamn\>*  och  *\<lösenord\>*  med användarnamn och lösenord för din katalog:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Gå till **C:\Program Files\Microsoft AAD App Proxy Connector** och kör skriptet med hjälp av PowerShell autentiseringsuppgifter objekt du har skapat. Ersätt *$cred* med namnet på PowerShell autentiseringsuppgifter objekt du har skapat:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a>Registrera anslutningsverktyget med hjälp av en token som skapats offline
1. Skapa en offline-token med hjälp av klassen AuthenticationContext med hjälp av värdena i kodfragmentet:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
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


2. När du har en token kan skapa en SecureString med token:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Kör följande Windows PowerShell-kommando ersätter \<klient GUID\> med katalog-ID:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Nästa steg 
* [Publicera program med ditt domännamn](active-directory-application-proxy-custom-domains.md)
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
* [Felsökning av problem med Application Proxy](active-directory-application-proxy-troubleshoot.md)


