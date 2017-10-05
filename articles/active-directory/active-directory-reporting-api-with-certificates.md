---
title: "Hämta data med hjälp av Azure AD Reporting API:et med certifikat | Microsoft Docs"
description: "Beskriver hur du använder Azure AD Reporting-API:et med certifikatautentiseringsuppgifter för att hämta data från kataloger utan inblandning av användaren."
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
ms.openlocfilehash: c1345dcda6e52267a8037ffd7207e6bc3b0d3b31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-data-using-the-azure-ad-reporting-api-with-certificates"></a><span data-ttu-id="fd8d3-103">Hämta data med hjälp av Azure AD Reporting-API:et med certifikat</span><span class="sxs-lookup"><span data-stu-id="fd8d3-103">Get data using the Azure AD Reporting API with certificates</span></span>
<span data-ttu-id="fd8d3-104">Den här artikeln beskriver hur du använder Azure AD Reporting-API:et med certifikatautentiseringsuppgifter för att hämta data från kataloger utan inblandning av användaren.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-104">This article discusses how to use the Azure AD Reporting API with certificate credentials to get data from directories without user intervention.</span></span> 

## <a name="use-the-azure-ad-reporting-api"></a><span data-ttu-id="fd8d3-105">Använda Azure AD Reporting-API:et</span><span class="sxs-lookup"><span data-stu-id="fd8d3-105">Use the Azure AD Reporting API</span></span> 
<span data-ttu-id="fd8d3-106">Azure AD Reporting-API:et kräver att du utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-106">Azure AD Reporting API requires that you complete the following steps:</span></span>
 *  <span data-ttu-id="fd8d3-107">Installationskrav</span><span class="sxs-lookup"><span data-stu-id="fd8d3-107">Install prerequisites</span></span>
 *  <span data-ttu-id="fd8d3-108">Konfigurera certifikatet i appen</span><span class="sxs-lookup"><span data-stu-id="fd8d3-108">Set the certificate in your app</span></span>
 *  <span data-ttu-id="fd8d3-109">Hämta en åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="fd8d3-109">Get an access token</span></span>
 *  <span data-ttu-id="fd8d3-110">Använd åtkomsttoken för att anropa Graph-API:et</span><span class="sxs-lookup"><span data-stu-id="fd8d3-110">Use the access token to call the Graph API</span></span>

<span data-ttu-id="fd8d3-111">Information om källkoden finns i [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils) (Använda Report API-modulen).</span><span class="sxs-lookup"><span data-stu-id="fd8d3-111">For information about source code, see [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span></span> 

### <a name="install-prerequisites"></a><span data-ttu-id="fd8d3-112">Installationskrav</span><span class="sxs-lookup"><span data-stu-id="fd8d3-112">Install prerequisites</span></span>
<span data-ttu-id="fd8d3-113">Azure AD PowerShell V2 och AzureADUtils-modulen måste vara installerade.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-113">You will need to have Azure AD PowerShell V2 and AzureADUtils module installed.</span></span>

1. <span data-ttu-id="fd8d3-114">Ladda ned och installera Azure AD Powershell V2 genom att följa anvisningarna i [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="fd8d3-114">Download and install Azure AD Powershell V2, following the instructions at [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span></span>
2. <span data-ttu-id="fd8d3-115">Ladda ned Azure AD Utils-modulen från [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span><span class="sxs-lookup"><span data-stu-id="fd8d3-115">Download the Azure AD Utils module from [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span></span> 
  <span data-ttu-id="fd8d3-116">Den här modulen tillhandahåller flera verktygs-cmdlets, däribland:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-116">This module provides several utility cmdlets including:</span></span>
   * <span data-ttu-id="fd8d3-117">Den senaste versionen av ADAL med Nuget</span><span class="sxs-lookup"><span data-stu-id="fd8d3-117">The latest version of ADAL using Nuget</span></span>
   * <span data-ttu-id="fd8d3-118">Åtkomsttoken från användare, programnycklar och certifikat med ADAL</span><span class="sxs-lookup"><span data-stu-id="fd8d3-118">Access tokens from user, application keys, and certificates using ADAL</span></span>
   * <span data-ttu-id="fd8d3-119">Växlingsbara resultat för Graph API-hantering</span><span class="sxs-lookup"><span data-stu-id="fd8d3-119">Graph API handling paged results</span></span>

<span data-ttu-id="fd8d3-120">**Så här installerar du Azure AD Utils-modulen:**</span><span class="sxs-lookup"><span data-stu-id="fd8d3-120">**To install the Azure AD Utils module:**</span></span>

1. <span data-ttu-id="fd8d3-121">Skapa en katalog för att spara verktygsmodulen (till exempel c:\azureAD) och ladda ned modulen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-121">Create a directory to save the utilities module (for example, c:\azureAD) and download the module from GitHub.</span></span>
2. <span data-ttu-id="fd8d3-122">Öppna en PowerShell-session och gå till den katalog som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-122">Open a PowerShell session, and go to the directory you just created.</span></span> 
3. <span data-ttu-id="fd8d3-123">Importera modulen och installera den på PowerShell-modulens sökväg med hjälp av cmdleten Install-AzureADUtilsModule.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-123">Import the module, and install it in the PowerShell module path using the Install-AzureADUtilsModule cmdlet.</span></span> 

<span data-ttu-id="fd8d3-124">Sessionen bör likna den här skärmen:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-124">The session should look similar to this screen:</span></span>

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-the-certificate-in-your-app"></a><span data-ttu-id="fd8d3-126">Konfigurera certifikatet i appen</span><span class="sxs-lookup"><span data-stu-id="fd8d3-126">Set the certificate in your app</span></span>
1. <span data-ttu-id="fd8d3-127">Om du redan har en app hämtar du dess objekt-ID från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-127">If you already have an app, get its Object ID from the Azure Portal.</span></span> 

  ![Azure Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. <span data-ttu-id="fd8d3-129">Öppna en PowerShell-session och anslut till Azure AD med hjälp av cmdleten Connect-AzureAD.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-129">Open a PowerShell session and connect to Azure AD using the Connect-AzureAD cmdlet.</span></span>

  ![Azure Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. <span data-ttu-id="fd8d3-131">Använd cmdleten New-AzureADApplicationCertificateCredential från AzureADUtils för att lägga till certifikatautentiseringsuppgifter till den.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-131">Use the New-AzureADApplicationCertificateCredential cmdlet from AzureADUtils to add a certificate credential to it.</span></span> 

>[!Note]
><span data-ttu-id="fd8d3-132">Du måste ange programmets objekt-ID som du hämtade tidigare, samt certifikatobjektet (som du hämtar med hjälp av Cert:-enheten).</span><span class="sxs-lookup"><span data-stu-id="fd8d3-132">You need to provide the application Object ID that you captured earlier, as well as the certificate object (get this using the Cert: drive).</span></span>
>


  ![Azure Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a><span data-ttu-id="fd8d3-134">Hämta en åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="fd8d3-134">Get an access token</span></span>

<span data-ttu-id="fd8d3-135">Du kommer åt token genom att använda cmdleten Get-AzureADGraphAPIAccessTokenFromCert från AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-135">To get an access token, use the Get-AzureADGraphAPIAccessTokenFromCert cmdlet from AzureADUtils.</span></span> 

>[!NOTE]
><span data-ttu-id="fd8d3-136">Du måste använda program-ID:t i stället för objekt-ID:t som du använde i det sista avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-136">You need to use the Application ID instead of the Object ID that you used in the last section.</span></span>
>

 ![Azure Portal](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-the-access-token-to-call-the-graph-api"></a><span data-ttu-id="fd8d3-138">Använd åtkomsttoken för att anropa Graph-API:et</span><span class="sxs-lookup"><span data-stu-id="fd8d3-138">Use the access token to call the Graph API</span></span>

<span data-ttu-id="fd8d3-139">Nu kan du skapa skriptet.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-139">Now you can create the script.</span></span> <span data-ttu-id="fd8d3-140">Nedan är ett exempel som använder cmdleten Invoke-AzureADGraphAPIQuery från AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-140">Below is an example using the Invoke-AzureADGraphAPIQuery cmdlet from the AzureADUtils.</span></span> <span data-ttu-id="fd8d3-141">Den här cmdleten hanterar flerväxlade resultat och skickar sedan dessa resultat till PowerShell-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-141">This cmdlet handles multi-paged results, and then sends those results to the PowerShell pipeline.</span></span> 

 ![Azure Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

<span data-ttu-id="fd8d3-143">Nu kan du exportera till en CSV-fil och spara till ett SIEM-system.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-143">You are now ready to export to a CSV and save to a SIEM system.</span></span> <span data-ttu-id="fd8d3-144">Du kan också ta med skriptet i en schemalagd aktivitet för att regelbundet hämta Azure AD-data från din klientorganisation utan att behöva lagra programnycklar i källkoden.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-144">You can also wrap your script in a scheduled task to get Azure AD data from your tenant periodically without having to store application keys in the source code.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fd8d3-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd8d3-145">Next steps</span></span>
[<span data-ttu-id="fd8d3-146">Grunderna i Azures identitetshantering</span><span class="sxs-lookup"><span data-stu-id="fd8d3-146">The fundamentals of Azure identity management</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



