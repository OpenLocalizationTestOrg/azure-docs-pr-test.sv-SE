---
title: "Azure Active Directory B2C: Användning reporting API samples och definitioner | Microsoft Docs"
description: "Guiden och exempel på att få rapporter om Azure AD B2C-klient användare, autentiseringar och Multi-Factor Authentication"
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: f01f0d1d12464e38f892f1fdd5c7408a3b4cd1aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-hello-reporting-api"></a>Åtkomst till användningsrapporter i Azure AD B2C via hello reporting API

Azure Active Directory B2C (Azure AD B2C) ger autentisering baserat på användarens inloggning och Azure Multi-Factor Authentication. Autentisering har angetts för slutanvändare i din familj program över identitetsleverantörer. När du vet hello antalet användare som har registrerats i hello klient hello providers de använde tooregister och hello Antal autentiseringar efter typ, kan du svara på frågor som:
* Hur många användare från varje typ av identitetsleverantör (till exempel en Microsoft eller LinkedIn-konto) har registrerats i hello senaste 10 dagarna
* Hur många autentiseringar med Multi-Factor Authentication har slutförts i hello förra månaden?
* Hur många tecken i baserade autentiseringar har slutfört den här månaden? Per dag? Per program?
* Hur kan jag beräkna hello förväntades månadskostnaden för aktiviteten min Azure AD B2C-klient?

Den här artikeln fokuserar på rapporter knutna toobilling aktivitet som baseras på hello antal användare, fakturerbar logga-baserat autentiseringar och Multi-Factor Authentication.


## <a name="prerequisites"></a>Krav
Innan du börjar behöver du toocomplete hello stegen i [krav tooaccess hello Azure AD reporting API: er](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/). Skapa ett program, skaffa en hemlighet för den och ge det åtkomst till rights tooyour Azure AD B2C-klient-rapporter. *Bash skriptet* och *Python-skriptet* exempel tillhandahålls också här. 

## <a name="powershell-script"></a>PowerShell-skript
Det här skriptet visar hello skapandet av fyra användningsrapporter med hjälp av hello `TimeStamp` parametern och hello `ApplicationId` filter.

```powershell
# This script will require hello Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"  
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.microsoftonline.com"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"  
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from hello tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for hello report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for hello " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a>Användning rapportdefinitioner
* **tenantUserCount**: hello antalet användare i hello innehavare av typ av identitetsleverantör per dag i hello senaste 30 dagarna. (Du kan också en `TimeStamp` filtret innehåller användaren antal från ett visst datum toohello aktuellt datum). hello rapporten innehåller:
  * **TotalUserCount**: hello antalet alla användarobjekt.
  * **OtherUserCount**: hello antalet Azure Active Directory-användare (inte Azure AD B2C-användare).
  * **LocalUserCount**: hello antal Azure AD B2C-användarkonton som skapats med autentiseringsuppgifter för lokal toohello Azure AD B2C-klient.

* **AlternateIdUserCount**: hello Azure AD B2C användare registrerats med externa identitetsleverantörer (till exempel Facebook, ett Microsoft-konto eller ett annat Azure Active Directory-klient, kallas även tooas en `OrgId`).

* **b2cAuthenticationCountSummary**: Sammanfattning av hello dagliga antalet fakturerbar autentiseringar över hello senaste 30 dagarna, per dag och typ av autentiseringsflödet.

* **b2cAuthenticationCount**: hello antalet autentiseringar inom en angiven tidsperiod. hello standardvärdet är hello senaste 30 dagarna.  (Valfritt: hello inledande och avslutande `TimeStamp` parametrar som definierar en specifik tidsperiod.) hello utdata innehåller `StartTimeStamp` (tidigaste datumet för aktiviteten för den här klienten) och `EndTimeStamp` (senaste uppdatering).

* **b2cMfaRequestCountSummary**: Sammanfattning av hello dagliga antalet Multi-Factor Authentication per dag och typ (SMS eller röst).


## <a name="limitations"></a>Begränsningar
Antal användardata uppdateras var 24 too48: e timme. Autentiseringar uppdateras flera gånger om dagen. När du använder hello `ApplicationId` filter, en tom rapport svaret kan vara på grund av tooone av följande villkor:
  * hello-ID: T finns inte i hello-klient. Kontrollera att den är korrekt.
  * hello-ID: T finns, men inga data hittades i hello rapporteringsperioden. Granska parametrarna datum/tid.


## <a name="next-steps"></a>Nästa steg
### <a name="monthly-bill-estimates-for-azure-ad"></a>Månadsfaktura beräknar för Azure AD
När den kombineras med [hello senaste Azure AD B2C priser tillgängliga](https://azure.microsoft.com/pricing/details/active-directory-b2c/), du kan beräkna dagliga, veckovisa och månatliga Azure-förbrukningen.  En uppskattning är särskilt användbart när du planerar för ändringar i klient-funktioner som kan påverka övergripande kostnader. Du kan granska faktiska kostnader i din [länkade Azure-prenumeration](active-directory-b2c-how-to-enable-billing.md).

### <a name="options-for-other-output-formats"></a>Alternativ för andra utdataformat
hello visar följande kod exempel för att skicka utdata tooJSON, en lista med namn på värden och XML:
```powershell
# toooutput tooJSON use following line in hello PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# toooutput hello content tooa name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# toooutput hello content in XML use hello following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
