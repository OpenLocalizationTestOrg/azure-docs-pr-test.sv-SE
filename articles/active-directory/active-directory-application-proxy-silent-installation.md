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
# <a name="silently-install-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="65356-103">Installera Azure AD Application Proxy Connector</span><span class="sxs-lookup"><span data-stu-id="65356-103">Silently install the Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="65356-104">Du vill kunna skicka ett skript för installation på flera Windows-servrar eller på Windows-servrar som inte har aktiverat användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="65356-104">You want to be able to send an installation script to multiple Windows servers or to Windows Servers that don't have user interface enabled.</span></span> <span data-ttu-id="65356-105">Det här avsnittet hjälper dig att skapa en Windows PowerShell-skript som aktiverar obevakad installation och registrering för din Azure AD Application Proxy Connector.</span><span class="sxs-lookup"><span data-stu-id="65356-105">This topic helps you create a Windows PowerShell script that enables unattended installation and registration for your Azure AD Application Proxy Connector.</span></span>

<span data-ttu-id="65356-106">Den här funktionen är användbar när du vill:</span><span class="sxs-lookup"><span data-stu-id="65356-106">This capability is useful when you want to:</span></span>

* <span data-ttu-id="65356-107">Installera connector på datorer utan användargränssnitt skikt eller när det går inte att RDP till datorn.</span><span class="sxs-lookup"><span data-stu-id="65356-107">Install the connector on machines with no UI layer or when you cannot RDP to the machine.</span></span>
* <span data-ttu-id="65356-108">Installera och registrera många kopplingar på samma gång.</span><span class="sxs-lookup"><span data-stu-id="65356-108">Install and register many connectors at once.</span></span>
* <span data-ttu-id="65356-109">Integrera kopplingsinstallationen och registrering som en del av en annan procedur.</span><span class="sxs-lookup"><span data-stu-id="65356-109">Integrate the connector installation and registration as part of another procedure.</span></span>
* <span data-ttu-id="65356-110">Skapa en standard serveravbildning som innehåller connector bitar men har inte registrerats.</span><span class="sxs-lookup"><span data-stu-id="65356-110">Create a standard server image that contains the connector bits but is not registered.</span></span>

<span data-ttu-id="65356-111">Programproxy fungerar genom att installera en smidig Windows Server-tjänsten som kallas kopplingen i nätverket.</span><span class="sxs-lookup"><span data-stu-id="65356-111">Application Proxy works by installing a slim Windows Server service called the Connector inside your network.</span></span> <span data-ttu-id="65356-112">Application Proxy Connector fungerar den har i registreras med Azure AD-katalogen med en global administratör och lösenord.</span><span class="sxs-lookup"><span data-stu-id="65356-112">For the Application Proxy Connector to work it has to be registered with your Azure AD directory using a global administrator and password.</span></span> <span data-ttu-id="65356-113">Den här informationen anges normalt under installationen av koppling i en dialogruta.</span><span class="sxs-lookup"><span data-stu-id="65356-113">Ordinarily this information is entered during Connector installation in a pop-up dialog box.</span></span> <span data-ttu-id="65356-114">Du kan dock använda Windows PowerShell för att skapa ett autentiseringsuppgiftobjekt för att ange din registreringsinformation.</span><span class="sxs-lookup"><span data-stu-id="65356-114">However, you can use Windows PowerShell to create a credential object to enter your registration information.</span></span> <span data-ttu-id="65356-115">Eller skapa egna token och använda den för att ange din registreringsinformation.</span><span class="sxs-lookup"><span data-stu-id="65356-115">Or you can create your own token and use it to enter your registration information.</span></span>

## <a name="install-the-connector"></a><span data-ttu-id="65356-116">Installera connector</span><span class="sxs-lookup"><span data-stu-id="65356-116">Install the connector</span></span>
<span data-ttu-id="65356-117">Installera Connector-MSI: er utan att registrera kopplingen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="65356-117">Install the Connector MSIs without registering the Connector as follows:</span></span>

1. <span data-ttu-id="65356-118">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="65356-118">Open a command prompt.</span></span>
2. <span data-ttu-id="65356-119">Kör följande kommando där /q innebär tyst installation - uppmanas installationen inte att godkänna licensavtalet.</span><span class="sxs-lookup"><span data-stu-id="65356-119">Run the following command in which the /q means quiet installation - the installation doesn't prompt you to accept the End-User License Agreement.</span></span>
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a><span data-ttu-id="65356-120">Registrera anslutningsverktyget med Azure AD</span><span class="sxs-lookup"><span data-stu-id="65356-120">Register the connector with Azure AD</span></span>
<span data-ttu-id="65356-121">Det finns två metoder som du kan använda för att registrera anslutningsverktyget:</span><span class="sxs-lookup"><span data-stu-id="65356-121">There are two methods you can use to register the connector:</span></span>

* <span data-ttu-id="65356-122">Registrera anslutningsverktyget med hjälp av Windows PowerShell-autentiseringsobjekt</span><span class="sxs-lookup"><span data-stu-id="65356-122">Register the connector using a Windows PowerShell credential object</span></span>
* <span data-ttu-id="65356-123">Registrera anslutningsverktyget med hjälp av en token som skapats offline</span><span class="sxs-lookup"><span data-stu-id="65356-123">Register the connector using a token created offline</span></span>

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a><span data-ttu-id="65356-124">Registrera anslutningsverktyget med hjälp av Windows PowerShell-autentiseringsobjekt</span><span class="sxs-lookup"><span data-stu-id="65356-124">Register the connector using a Windows PowerShell credential object</span></span>
1. <span data-ttu-id="65356-125">Skapa Windows PowerShell-autentiseringsuppgifter objektet genom att köra det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="65356-125">Create the Windows PowerShell Credentials object by running this command.</span></span> <span data-ttu-id="65356-126">Ersätt  *\<användarnamn\>*  och  *\<lösenord\>*  med användarnamn och lösenord för din katalog:</span><span class="sxs-lookup"><span data-stu-id="65356-126">Replace *\<username\>* and *\<password\>* with the username and password for your directory:</span></span>
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. <span data-ttu-id="65356-127">Gå till **C:\Program Files\Microsoft AAD App Proxy Connector** och kör skriptet med hjälp av PowerShell autentiseringsuppgifter objekt du har skapat.</span><span class="sxs-lookup"><span data-stu-id="65356-127">Go to **C:\Program Files\Microsoft AAD App Proxy Connector** and run the script using the PowerShell credentials object you created.</span></span> <span data-ttu-id="65356-128">Ersätt *$cred* med namnet på PowerShell autentiseringsuppgifter objekt du har skapat:</span><span class="sxs-lookup"><span data-stu-id="65356-128">Replace *$cred* with the name of the PowerShell credentials object you created:</span></span>
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a><span data-ttu-id="65356-129">Registrera anslutningsverktyget med hjälp av en token som skapats offline</span><span class="sxs-lookup"><span data-stu-id="65356-129">Register the connector using a token created offline</span></span>
1. <span data-ttu-id="65356-130">Skapa en offline-token med hjälp av klassen AuthenticationContext med hjälp av värdena i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="65356-130">Create an offline token using the AuthenticationContext class using the values in the code snippet:</span></span>

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


2. <span data-ttu-id="65356-131">När du har en token kan skapa en SecureString med token:</span><span class="sxs-lookup"><span data-stu-id="65356-131">Once you have the token, create a SecureString using the token:</span></span>

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. <span data-ttu-id="65356-132">Kör följande Windows PowerShell-kommando ersätter \<klient GUID\> med katalog-ID:</span><span class="sxs-lookup"><span data-stu-id="65356-132">Run the following Windows PowerShell command, replacing \<tenant GUID\> with your directory ID:</span></span>

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a><span data-ttu-id="65356-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="65356-133">Next steps</span></span> 
* [<span data-ttu-id="65356-134">Publicera program med ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="65356-134">Publish applications using your own domain name</span></span>](active-directory-application-proxy-custom-domains.md)
* [<span data-ttu-id="65356-135">Aktivera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65356-135">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="65356-136">Felsökning av problem med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="65356-136">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)


