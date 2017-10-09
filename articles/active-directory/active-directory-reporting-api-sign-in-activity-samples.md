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
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory inloggningsaktivitet rapporten API-exempel
Det här avsnittet är en del av en samling ämnen om hello Azure Active Directory reporting API.  
Azure AD-rapportering ger dig en API som gör att du tooaccess inloggningsaktivitet data med hjälp av koden eller relaterade verktyg.  
hello omfånget för det här avsnittet är tooprovide som till exempel koden för hello **aktivitet API inloggning**.

Se:

* [Granskningsloggar](active-directory-reporting-azure-portal.md#activity-reports) mer information
* [Komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) mer information om hello reporting API.


## <a name="prerequisites"></a>Krav
Innan du kan använda hello prover i det här avsnittet, behöver du toocomplete hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>PowerShell-skript
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




## <a name="executing-hello-script"></a>Skriptet hello
När du hello skriptet är klar, kör den och verifiera att hello förväntade data från hello loggar kontrollrapport returneras.

hello skriptet returnerar utdata från hello inloggning rapport i JSON-format. Det skapar också ett `SigninActivities.json` filen med hello samma utdata. Du kan experimentera genom att ändra hello skriptet tooreturn data från andra rapporter och kommentera ut hello utdataformat som du inte behöver.

## <a name="next-steps"></a>Nästa steg
* Vill du toocustomize hello prover i det här avsnittet? Kolla in hello [Azure Active Directory inloggningsaktivitet API-referens för](active-directory-reporting-api-sign-in-activity-reference.md). 
* Om du vill toosee en fullständig översikt över användning av hello Azure Active Directory reporting API, se [komma igång med hello Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).
* Om du vill toofind mer information om Azure Active Directory reporting finns hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  

