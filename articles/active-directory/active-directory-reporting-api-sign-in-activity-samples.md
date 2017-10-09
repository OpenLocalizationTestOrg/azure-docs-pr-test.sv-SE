---
title: aaaAzure Active Directory-inloggning aktivitet rapporten API exempel | Microsoft Docs
description: "Hur tooget igång med hello Azure Active Directory Reporting API"
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
ms.openlocfilehash: d4fbbea95fe0b52828673b997681ae37481e21bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a><span data-ttu-id="f682d-103">Azure Active Directory inloggningsaktivitet rapporten API-exempel</span><span class="sxs-lookup"><span data-stu-id="f682d-103">Azure Active Directory sign-in activity report API samples</span></span>
<span data-ttu-id="f682d-104">Det här avsnittet är en del av en samling ämnen om hello Azure Active Directory reporting API.</span><span class="sxs-lookup"><span data-stu-id="f682d-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="f682d-105">Azure AD-rapportering ger dig en API som gör att du tooaccess inloggningsaktivitet data med hjälp av koden eller relaterade verktyg.</span><span class="sxs-lookup"><span data-stu-id="f682d-105">Azure AD reporting provides you with an API that enables you tooaccess sign-in activity data using code or related tools.</span></span>  
<span data-ttu-id="f682d-106">hello omfånget för det här avsnittet är tooprovide som till exempel koden för hello **aktivitet API inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f682d-106">hello scope of this topic is tooprovide you with sample code for hello **sign-in activity API**.</span></span>

<span data-ttu-id="f682d-107">Se:</span><span class="sxs-lookup"><span data-stu-id="f682d-107">See:</span></span>

* <span data-ttu-id="f682d-108">[Granskningsloggar](active-directory-reporting-azure-portal.md#activity-reports) mer information</span><span class="sxs-lookup"><span data-stu-id="f682d-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>
* <span data-ttu-id="f682d-109">[Komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) mer information om hello reporting API.</span><span class="sxs-lookup"><span data-stu-id="f682d-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f682d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f682d-110">Prerequisites</span></span>
<span data-ttu-id="f682d-111">Innan du kan använda hello prover i det här avsnittet, behöver du toocomplete hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="f682d-111">Before you can use hello samples in this topic, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>  

## <a name="powershell-script"></a><span data-ttu-id="f682d-112">PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="f682d-112">PowerShell script</span></span>
    # This script will require hello Web Application and permissions setup in Azure Active Directory
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
        Write-Output "Save hello output tooa file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-hello-script"></a><span data-ttu-id="f682d-113">Skriptet hello</span><span class="sxs-lookup"><span data-stu-id="f682d-113">Executing hello script</span></span>
<span data-ttu-id="f682d-114">När du hello skriptet är klar, kör den och verifiera att hello förväntade data från hello loggar kontrollrapport returneras.</span><span class="sxs-lookup"><span data-stu-id="f682d-114">Once you finish editing hello script, run it and verify that hello expected data from hello Audit logs report is returned.</span></span>

<span data-ttu-id="f682d-115">hello skriptet returnerar utdata från hello inloggning rapport i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="f682d-115">hello script returns output from hello sign-in report in JSON format.</span></span> <span data-ttu-id="f682d-116">Det skapar också ett `SigninActivities.json` filen med hello samma utdata.</span><span class="sxs-lookup"><span data-stu-id="f682d-116">It also creates an `SigninActivities.json` file with hello same output.</span></span> <span data-ttu-id="f682d-117">Du kan experimentera genom att ändra hello skriptet tooreturn data från andra rapporter och kommentera ut hello utdataformat som du inte behöver.</span><span class="sxs-lookup"><span data-stu-id="f682d-117">You can experiment by modifying hello script tooreturn data from other reports, and comment out hello output formats that you do not need.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f682d-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f682d-118">Next Steps</span></span>
* <span data-ttu-id="f682d-119">Vill du toocustomize hello prover i det här avsnittet?</span><span class="sxs-lookup"><span data-stu-id="f682d-119">Would you like toocustomize hello samples in this topic?</span></span> <span data-ttu-id="f682d-120">Kolla in hello [Azure Active Directory inloggningsaktivitet API-referens för](active-directory-reporting-api-sign-in-activity-reference.md).</span><span class="sxs-lookup"><span data-stu-id="f682d-120">Check out hello [Azure Active Directory sign-in activity API reference](active-directory-reporting-api-sign-in-activity-reference.md).</span></span> 
* <span data-ttu-id="f682d-121">Om du vill toosee en fullständig översikt över användning av hello Azure Active Directory reporting API, se [komma igång med hello Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f682d-121">If you want toosee a complete overview of using hello Azure Active Directory reporting API, see [Getting started with hello Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="f682d-122">Om du vill toofind mer information om Azure Active Directory reporting finns hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="f682d-122">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

