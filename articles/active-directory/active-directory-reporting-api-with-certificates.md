---
title: "aaaGet data med hjälp av hello Azure AD Reporting API med certifikat | Microsoft Docs"
description: "Förklarar hur toouse hello Azure AD Reporting API med certifikatet autentiseringsuppgifter tooget data från kataloger utan åtgärder från användaren."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2017
ms.author: ramical
ms.openlocfilehash: 00ddfaefe32ea6ae48f276c974a17ddcf84f7894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-data-using-hello-azure-ad-reporting-api-with-certificates"></a><span data-ttu-id="feb80-103">Hämta data med certifikat hello Azure AD Reporting API</span><span class="sxs-lookup"><span data-stu-id="feb80-103">Get data using hello Azure AD Reporting API with certificates</span></span>
<span data-ttu-id="feb80-104">Den här artikeln beskrivs hur toouse hello Azure AD Reporting API med certifikatet autentiseringsuppgifter tooget data från kataloger utan åtgärder från användaren.</span><span class="sxs-lookup"><span data-stu-id="feb80-104">This article discusses how toouse hello Azure AD Reporting API with certificate credentials tooget data from directories without user intervention.</span></span> 

## <a name="use-hello-azure-ad-reporting-api"></a><span data-ttu-id="feb80-105">Använd hello Azure AD Reporting API</span><span class="sxs-lookup"><span data-stu-id="feb80-105">Use hello Azure AD Reporting API</span></span> 
<span data-ttu-id="feb80-106">Azure AD Reporting API kräver att du har slutfört hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="feb80-106">Azure AD Reporting API requires that you complete hello following steps:</span></span>
 *  <span data-ttu-id="feb80-107">Installationskrav</span><span class="sxs-lookup"><span data-stu-id="feb80-107">Install prerequisites</span></span>
 *  <span data-ttu-id="feb80-108">Ange hello certifikat i din app</span><span class="sxs-lookup"><span data-stu-id="feb80-108">Set hello certificate in your app</span></span>
 *  <span data-ttu-id="feb80-109">Hämta en åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="feb80-109">Get an access token</span></span>
 *  <span data-ttu-id="feb80-110">Använd hello åtkomst-token toocall hello Graph API</span><span class="sxs-lookup"><span data-stu-id="feb80-110">Use hello access token toocall hello Graph API</span></span>

<span data-ttu-id="feb80-111">Information om källkoden finns i [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils) (Använda Report API-modulen).</span><span class="sxs-lookup"><span data-stu-id="feb80-111">For information about source code, see [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span></span> 

### <a name="install-prerequisites"></a><span data-ttu-id="feb80-112">Installationskrav</span><span class="sxs-lookup"><span data-stu-id="feb80-112">Install prerequisites</span></span>
<span data-ttu-id="feb80-113">Du behöver toohave Azure AD PowerShell V2 och AzureADUtils module installerad.</span><span class="sxs-lookup"><span data-stu-id="feb80-113">You will need toohave Azure AD PowerShell V2 and AzureADUtils module installed.</span></span>

1. <span data-ttu-id="feb80-114">Hämta och installera Azure AD Powershell V2, hello instruktionerna på [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="feb80-114">Download and install Azure AD Powershell V2, following hello instructions at [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span></span>
2. <span data-ttu-id="feb80-115">Hämta hello verktyg för Azure AD webbplatsuppgradering modul från [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span><span class="sxs-lookup"><span data-stu-id="feb80-115">Download hello Azure AD Utils module from [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span></span> 
  <span data-ttu-id="feb80-116">Den här modulen tillhandahåller flera verktygs-cmdlets, däribland:</span><span class="sxs-lookup"><span data-stu-id="feb80-116">This module provides several utility cmdlets including:</span></span>
   * <span data-ttu-id="feb80-117">hello senaste versionen av ADAL med hjälp av Nuget</span><span class="sxs-lookup"><span data-stu-id="feb80-117">hello latest version of ADAL using Nuget</span></span>
   * <span data-ttu-id="feb80-118">Åtkomsttoken från användare, programnycklar och certifikat med ADAL</span><span class="sxs-lookup"><span data-stu-id="feb80-118">Access tokens from user, application keys, and certificates using ADAL</span></span>
   * <span data-ttu-id="feb80-119">Växlingsbara resultat för Graph API-hantering</span><span class="sxs-lookup"><span data-stu-id="feb80-119">Graph API handling paged results</span></span>

<span data-ttu-id="feb80-120">**tooinstall hello verktyg för Azure AD webbplatsuppgradering modul:**</span><span class="sxs-lookup"><span data-stu-id="feb80-120">**tooinstall hello Azure AD Utils module:**</span></span>

1. <span data-ttu-id="feb80-121">Skapa en katalog toosave hello verktyg modul (till exempel c:\azureAD) och hämta hello modul från GitHub.</span><span class="sxs-lookup"><span data-stu-id="feb80-121">Create a directory toosave hello utilities module (for example, c:\azureAD) and download hello module from GitHub.</span></span>
2. <span data-ttu-id="feb80-122">Öppna ett PowerShell-session och gå toohello katalog som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="feb80-122">Open a PowerShell session, and go toohello directory you just created.</span></span> 
3. <span data-ttu-id="feb80-123">Importera hello-modulen och installera den i hello PowerShell modul-sökväg med hello installera AzureADUtilsModule cmdlet.</span><span class="sxs-lookup"><span data-stu-id="feb80-123">Import hello module, and install it in hello PowerShell module path using hello Install-AzureADUtilsModule cmdlet.</span></span> 

<span data-ttu-id="feb80-124">hello session bör se ut ungefär toothis skärm:</span><span class="sxs-lookup"><span data-stu-id="feb80-124">hello session should look similar toothis screen:</span></span>

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-hello-certificate-in-your-app"></a><span data-ttu-id="feb80-126">Ange hello certifikat i din app</span><span class="sxs-lookup"><span data-stu-id="feb80-126">Set hello certificate in your app</span></span>
1. <span data-ttu-id="feb80-127">Om du redan har en app kan du hämta dess objekt-ID från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="feb80-127">If you already have an app, get its Object ID from hello Azure Portal.</span></span> 

  ![Azure Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. <span data-ttu-id="feb80-129">Öppna ett PowerShell-session och Anslut tooAzure AD med hello Connect-AzureAD cmdlet.</span><span class="sxs-lookup"><span data-stu-id="feb80-129">Open a PowerShell session and connect tooAzure AD using hello Connect-AzureAD cmdlet.</span></span>

  ![Azure Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. <span data-ttu-id="feb80-131">Använd hello ny AzureADApplicationCertificateCredential cmdlet från AzureADUtils tooadd tooit en certifikat-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="feb80-131">Use hello New-AzureADApplicationCertificateCredential cmdlet from AzureADUtils tooadd a certificate credential tooit.</span></span> 

>[!Note]
><span data-ttu-id="feb80-132">Du behöver tooprovide hello programmet objekt-ID som du hämtat tidigare, samt hello certifikatobjekt (hämta detta med hjälp av hello Cert: enheten).</span><span class="sxs-lookup"><span data-stu-id="feb80-132">You need tooprovide hello application Object ID that you captured earlier, as well as hello certificate object (get this using hello Cert: drive).</span></span>
>


  ![Azure Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a><span data-ttu-id="feb80-134">Hämta en åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="feb80-134">Get an access token</span></span>

<span data-ttu-id="feb80-135">tooget ett åtkomsttoken, Använd hello Get-AzureADGraphAPIAccessTokenFromCert cmdlet från AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="feb80-135">tooget an access token, use hello Get-AzureADGraphAPIAccessTokenFromCert cmdlet from AzureADUtils.</span></span> 

>[!NOTE]
><span data-ttu-id="feb80-136">Du måste toouse hello program-ID i stället för hello objekt-ID som du använde i hello sista avsnittet.</span><span class="sxs-lookup"><span data-stu-id="feb80-136">You need toouse hello Application ID instead of hello Object ID that you used in hello last section.</span></span>
>

 ![Azure Portal](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-hello-access-token-toocall-hello-graph-api"></a><span data-ttu-id="feb80-138">Använd hello åtkomst-token toocall hello Graph API</span><span class="sxs-lookup"><span data-stu-id="feb80-138">Use hello access token toocall hello Graph API</span></span>

<span data-ttu-id="feb80-139">Du kan nu skapa hello skript.</span><span class="sxs-lookup"><span data-stu-id="feb80-139">Now you can create hello script.</span></span> <span data-ttu-id="feb80-140">Nedan visas ett exempel med hello Invoke-AzureADGraphAPIQuery cmdlet från hello AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="feb80-140">Below is an example using hello Invoke-AzureADGraphAPIQuery cmdlet from hello AzureADUtils.</span></span> <span data-ttu-id="feb80-141">Denna cmdlet hanterar flera växlingsbara resultat, och skickar sedan dessa resultat toohello PowerShell-pipeline.</span><span class="sxs-lookup"><span data-stu-id="feb80-141">This cmdlet handles multi-paged results, and then sends those results toohello PowerShell pipeline.</span></span> 

 ![Azure Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

<span data-ttu-id="feb80-143">Du är nu redo tooexport tooa CSV och spara tooa SIEM-system.</span><span class="sxs-lookup"><span data-stu-id="feb80-143">You are now ready tooexport tooa CSV and save tooa SIEM system.</span></span> <span data-ttu-id="feb80-144">Du kan också omsluta skriptet i en schemalagd aktivitet tooget Azure AD-data från din klient regelbundet utan toostore programnycklar i hello källkod.</span><span class="sxs-lookup"><span data-stu-id="feb80-144">You can also wrap your script in a scheduled task tooget Azure AD data from your tenant periodically without having toostore application keys in hello source code.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="feb80-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="feb80-145">Next steps</span></span>
[<span data-ttu-id="feb80-146">hello grunderna i Azure Identitetshantering</span><span class="sxs-lookup"><span data-stu-id="feb80-146">hello fundamentals of Azure identity management</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



