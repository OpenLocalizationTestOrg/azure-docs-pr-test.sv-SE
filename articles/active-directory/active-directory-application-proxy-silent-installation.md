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
# <a name="silently-install-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="f1bb5-103">Obevakad installation hello Azure AD Application Proxy Connector</span><span class="sxs-lookup"><span data-stu-id="f1bb5-103">Silently install hello Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="f1bb5-104">Vill du toobe kan toosend en installation skript toomultiple Windows eller tooWindows servrar som inte har aktiverat användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-104">You want toobe able toosend an installation script toomultiple Windows servers or tooWindows Servers that don't have user interface enabled.</span></span> <span data-ttu-id="f1bb5-105">Det här avsnittet hjälper dig att skapa en Windows PowerShell-skript som aktiverar obevakad installation och registrering för din Azure AD Application Proxy Connector.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-105">This topic helps you create a Windows PowerShell script that enables unattended installation and registration for your Azure AD Application Proxy Connector.</span></span>

<span data-ttu-id="f1bb5-106">Den här funktionen är användbar när du vill:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-106">This capability is useful when you want to:</span></span>

* <span data-ttu-id="f1bb5-107">Installera hello connector på datorer utan användargränssnitt skikt eller när det går inte att RDP toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-107">Install hello connector on machines with no UI layer or when you cannot RDP toohello machine.</span></span>
* <span data-ttu-id="f1bb5-108">Installera och registrera många kopplingar på samma gång.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-108">Install and register many connectors at once.</span></span>
* <span data-ttu-id="f1bb5-109">Integrera hello connector installation och registrering som en del av en annan procedur.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-109">Integrate hello connector installation and registration as part of another procedure.</span></span>
* <span data-ttu-id="f1bb5-110">Skapa en standard serveravbildning som innehåller hello connector bitar men har inte registrerats.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-110">Create a standard server image that contains hello connector bits but is not registered.</span></span>

<span data-ttu-id="f1bb5-111">Programproxy fungerar genom att installera en smidig Windows Server-tjänsten som kallas hello Connector i ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-111">Application Proxy works by installing a slim Windows Server service called hello Connector inside your network.</span></span> <span data-ttu-id="f1bb5-112">Hello Application Proxy Connector toowork har toobe registrerad med Azure AD-katalogen med en global administratör och lösenord.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-112">For hello Application Proxy Connector toowork it has toobe registered with your Azure AD directory using a global administrator and password.</span></span> <span data-ttu-id="f1bb5-113">Den här informationen anges normalt under installationen av koppling i en dialogruta.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-113">Ordinarily this information is entered during Connector installation in a pop-up dialog box.</span></span> <span data-ttu-id="f1bb5-114">Du kan dock använda Windows PowerShell toocreate en autentiseringsuppgift objektet tooenter registreringsinformationen.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-114">However, you can use Windows PowerShell toocreate a credential object tooenter your registration information.</span></span> <span data-ttu-id="f1bb5-115">Du kan skapa egna token och använda den tooenter registreringsinformationen.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-115">Or you can create your own token and use it tooenter your registration information.</span></span>

## <a name="install-hello-connector"></a><span data-ttu-id="f1bb5-116">Installera hello connector</span><span class="sxs-lookup"><span data-stu-id="f1bb5-116">Install hello connector</span></span>
<span data-ttu-id="f1bb5-117">Installera hello Connector MSI: er utan att registrera hello Connector på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-117">Install hello Connector MSIs without registering hello Connector as follows:</span></span>

1. <span data-ttu-id="f1bb5-118">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-118">Open a command prompt.</span></span>
2. <span data-ttu-id="f1bb5-119">Kör följande kommando i vilka hello /q innebär tyst installation hello - hello installationen uppmanas inte tooaccept hello licensavtal (EULA).</span><span class="sxs-lookup"><span data-stu-id="f1bb5-119">Run hello following command in which hello /q means quiet installation - hello installation doesn't prompt you tooaccept hello End-User License Agreement.</span></span>
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-hello-connector-with-azure-ad"></a><span data-ttu-id="f1bb5-120">Registrera hello connector med Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1bb5-120">Register hello connector with Azure AD</span></span>
<span data-ttu-id="f1bb5-121">Det finns två metoder som du kan använda tooregister hello anslutningen:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-121">There are two methods you can use tooregister hello connector:</span></span>

* <span data-ttu-id="f1bb5-122">Registrera hello connector med hjälp av Windows PowerShell-autentiseringsobjekt</span><span class="sxs-lookup"><span data-stu-id="f1bb5-122">Register hello connector using a Windows PowerShell credential object</span></span>
* <span data-ttu-id="f1bb5-123">Registrera hello connector med hjälp av en token som skapats offline</span><span class="sxs-lookup"><span data-stu-id="f1bb5-123">Register hello connector using a token created offline</span></span>

### <a name="register-hello-connector-using-a-windows-powershell-credential-object"></a><span data-ttu-id="f1bb5-124">Registrera hello connector med hjälp av Windows PowerShell-autentiseringsobjekt</span><span class="sxs-lookup"><span data-stu-id="f1bb5-124">Register hello connector using a Windows PowerShell credential object</span></span>
1. <span data-ttu-id="f1bb5-125">Skapa hello Windows PowerShell-autentiseringsuppgifter objekt genom att köra det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-125">Create hello Windows PowerShell Credentials object by running this command.</span></span> <span data-ttu-id="f1bb5-126">Ersätt  *\<användarnamn\>*  och  *\<lösenord\>*  med hello användarnamn och lösenord för din katalog:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-126">Replace *\<username\>* and *\<password\>* with hello username and password for your directory:</span></span>
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. <span data-ttu-id="f1bb5-127">Gå för**C:\Program Files\Microsoft AAD App Proxy Connector** och köra hello skript med hello PowerShell autentiseringsuppgifter objekt du har skapat.</span><span class="sxs-lookup"><span data-stu-id="f1bb5-127">Go too**C:\Program Files\Microsoft AAD App Proxy Connector** and run hello script using hello PowerShell credentials object you created.</span></span> <span data-ttu-id="f1bb5-128">Ersätt *$cred* med hello namnet hello PowerShell autentiseringsuppgifter objekt du har skapat:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-128">Replace *$cred* with hello name of hello PowerShell credentials object you created:</span></span>
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-hello-connector-using-a-token-created-offline"></a><span data-ttu-id="f1bb5-129">Registrera hello connector med hjälp av en token som skapats offline</span><span class="sxs-lookup"><span data-stu-id="f1bb5-129">Register hello connector using a token created offline</span></span>
1. <span data-ttu-id="f1bb5-130">Skapa en offline-token med hello värden i hello kodstycke hello AuthenticationContext-klassen:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-130">Create an offline token using hello AuthenticationContext class using hello values in hello code snippet:</span></span>

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


2. <span data-ttu-id="f1bb5-131">När du har hello token kan skapa en SecureString använder hello-token:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-131">Once you have hello token, create a SecureString using hello token:</span></span>

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. <span data-ttu-id="f1bb5-132">Kör hello följande Windows PowerShell-kommandot, ersätter \<klient GUID\> med katalog-ID:</span><span class="sxs-lookup"><span data-stu-id="f1bb5-132">Run hello following Windows PowerShell command, replacing \<tenant GUID\> with your directory ID:</span></span>

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a><span data-ttu-id="f1bb5-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1bb5-133">Next steps</span></span> 
* [<span data-ttu-id="f1bb5-134">Publicera program med ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="f1bb5-134">Publish applications using your own domain name</span></span>](active-directory-application-proxy-custom-domains.md)
* [<span data-ttu-id="f1bb5-135">Aktivera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f1bb5-135">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="f1bb5-136">Felsökning av problem med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="f1bb5-136">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)


