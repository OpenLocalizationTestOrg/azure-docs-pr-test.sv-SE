---
title: Azure Active Directory inloggningsaktivitet rapporten API-exempel | Microsoft Docs
description: "Hur du kommer igång med Azure Active Directory Reporting API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 7fc2b59fe37ed2ffe85925c457300ef8fd83c3c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a><span data-ttu-id="d10cf-103">Azure Active Directory inloggningsaktivitet rapporten API-exempel</span><span class="sxs-lookup"><span data-stu-id="d10cf-103">Azure Active Directory sign-in activity report API samples</span></span>
<span data-ttu-id="d10cf-104">Det här avsnittet är en del av en samling ämnen om Azure Active Directory reporting API.</span><span class="sxs-lookup"><span data-stu-id="d10cf-104">This topic is part of a collection of topics about the Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="d10cf-105">Azure AD-rapportering ger dig en API som gör att du kan komma åt inloggningsaktivitet data med hjälp av koden eller relaterade verktyg.</span><span class="sxs-lookup"><span data-stu-id="d10cf-105">Azure AD reporting provides you with an API that enables you to access sign-in activity data using code or related tools.</span></span>  
<span data-ttu-id="d10cf-106">Omfattningen av det här avsnittet är att ge dig exempelkod för den **aktivitet API inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d10cf-106">The scope of this topic is to provide you with sample code for the **sign-in activity API**.</span></span>

<span data-ttu-id="d10cf-107">Se:</span><span class="sxs-lookup"><span data-stu-id="d10cf-107">See:</span></span>

* <span data-ttu-id="d10cf-108">[Granskningsloggar](active-directory-reporting-azure-portal.md#activity-reports) mer information</span><span class="sxs-lookup"><span data-stu-id="d10cf-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>
* <span data-ttu-id="d10cf-109">[Komma igång med Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) för mer information om reporting API.</span><span class="sxs-lookup"><span data-stu-id="d10cf-109">[Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about the reporting API.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="d10cf-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d10cf-110">Prerequisites</span></span>
<span data-ttu-id="d10cf-111">Innan du kan använda exemplen i det här avsnittet, måste du slutföra de [krav för att få åtkomst till Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="d10cf-111">Before you can use the samples in this topic, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>  

## <a name="powershell-script"></a><span data-ttu-id="d10cf-112">PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="d10cf-112">PowerShell script</span></span>
    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.microsoftonline.com/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"

    $i=0

    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a><span data-ttu-id="d10cf-113">Körning av skriptet</span><span class="sxs-lookup"><span data-stu-id="d10cf-113">Executing the script</span></span>
<span data-ttu-id="d10cf-114">När du har redigerat skriptet körs och kontrollera att de förväntade data från revision loggar rapporten returneras.</span><span class="sxs-lookup"><span data-stu-id="d10cf-114">Once you finish editing the script, run it and verify that the expected data from the Audit logs report is returned.</span></span>

<span data-ttu-id="d10cf-115">Skriptet returnerar utdata från rapporten i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="d10cf-115">The script returns output from the sign-in report in JSON format.</span></span> <span data-ttu-id="d10cf-116">Det skapar också en `SigninActivities.json` fil med samma utdata.</span><span class="sxs-lookup"><span data-stu-id="d10cf-116">It also creates an `SigninActivities.json` file with the same output.</span></span> <span data-ttu-id="d10cf-117">Du kan experimentera genom att ändra skriptet för att returnera data från andra rapporter och kommentera ut utdataformat som du inte behöver.</span><span class="sxs-lookup"><span data-stu-id="d10cf-117">You can experiment by modifying the script to return data from other reports, and comment out the output formats that you do not need.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d10cf-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d10cf-118">Next Steps</span></span>
* <span data-ttu-id="d10cf-119">Vill du anpassa exemplen i det här avsnittet?</span><span class="sxs-lookup"><span data-stu-id="d10cf-119">Would you like to customize the samples in this topic?</span></span> <span data-ttu-id="d10cf-120">Kolla in den [Azure Active Directory inloggningsaktivitet API-referens för](active-directory-reporting-api-sign-in-activity-reference.md).</span><span class="sxs-lookup"><span data-stu-id="d10cf-120">Check out the [Azure Active Directory sign-in activity API reference](active-directory-reporting-api-sign-in-activity-reference.md).</span></span> 
* <span data-ttu-id="d10cf-121">Om du vill se en fullständig översikt över med Azure Active Directory reporting API, se [komma igång med Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d10cf-121">If you want to see a complete overview of using the Azure Active Directory reporting API, see [Getting started with the Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="d10cf-122">Om du vill veta mer om Azure Active Directory reporting finns i [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="d10cf-122">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

