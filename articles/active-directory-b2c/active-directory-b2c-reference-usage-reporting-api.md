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
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-hello-reporting-api"></a><span data-ttu-id="55a26-103">Åtkomst till användningsrapporter i Azure AD B2C via hello reporting API</span><span class="sxs-lookup"><span data-stu-id="55a26-103">Accessing usage reports in Azure AD B2C via hello reporting API</span></span>

<span data-ttu-id="55a26-104">Azure Active Directory B2C (Azure AD B2C) ger autentisering baserat på användarens inloggning och Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="55a26-104">Azure Active Directory B2C (Azure AD B2C) provides authentication based on user sign-in and Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="55a26-105">Autentisering har angetts för slutanvändare i din familj program över identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="55a26-105">Authentication is provided for end users of your application family across identity providers.</span></span> <span data-ttu-id="55a26-106">När du vet hello antalet användare som har registrerats i hello klient hello providers de använde tooregister och hello Antal autentiseringar efter typ, kan du svara på frågor som:</span><span class="sxs-lookup"><span data-stu-id="55a26-106">When you know hello number of users registered in hello tenant, hello providers they used tooregister, and hello number of authentications by type, you can answer questions like:</span></span>
* <span data-ttu-id="55a26-107">Hur många användare från varje typ av identitetsleverantör (till exempel en Microsoft eller LinkedIn-konto) har registrerats i hello senaste 10 dagarna</span><span class="sxs-lookup"><span data-stu-id="55a26-107">How many users from each type of identity provider (for example, a Microsoft or LinkedIn account) have registered in hello last 10 days?</span></span>
* <span data-ttu-id="55a26-108">Hur många autentiseringar med Multi-Factor Authentication har slutförts i hello förra månaden?</span><span class="sxs-lookup"><span data-stu-id="55a26-108">How many authentications using Multi-Factor Authentication have completed successfully in hello last month?</span></span>
* <span data-ttu-id="55a26-109">Hur många tecken i baserade autentiseringar har slutfört den här månaden?</span><span class="sxs-lookup"><span data-stu-id="55a26-109">How many sign-in-based authentications were completed this month?</span></span> <span data-ttu-id="55a26-110">Per dag?</span><span class="sxs-lookup"><span data-stu-id="55a26-110">Per day?</span></span> <span data-ttu-id="55a26-111">Per program?</span><span class="sxs-lookup"><span data-stu-id="55a26-111">Per application?</span></span>
* <span data-ttu-id="55a26-112">Hur kan jag beräkna hello förväntades månadskostnaden för aktiviteten min Azure AD B2C-klient?</span><span class="sxs-lookup"><span data-stu-id="55a26-112">How can I estimate hello expected monthly cost of my Azure AD B2C tenant activity?</span></span>

<span data-ttu-id="55a26-113">Den här artikeln fokuserar på rapporter knutna toobilling aktivitet som baseras på hello antal användare, fakturerbar logga-baserat autentiseringar och Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="55a26-113">This article focuses on reports tied toobilling activity, which is based on hello number of users, billable sign-in-based authentications, and multi-factor authentications.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="55a26-114">Krav</span><span class="sxs-lookup"><span data-stu-id="55a26-114">Prerequisites</span></span>
<span data-ttu-id="55a26-115">Innan du börjar behöver du toocomplete hello stegen i [krav tooaccess hello Azure AD reporting API: er](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="55a26-115">Before you get started, you need toocomplete hello steps in [Prerequisites tooaccess hello Azure AD reporting APIs](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span></span> <span data-ttu-id="55a26-116">Skapa ett program, skaffa en hemlighet för den och ge det åtkomst till rights tooyour Azure AD B2C-klient-rapporter.</span><span class="sxs-lookup"><span data-stu-id="55a26-116">Create an application, obtain a secret for it, and grant it access rights tooyour Azure AD B2C tenant’s reports.</span></span> <span data-ttu-id="55a26-117">*Bash skriptet* och *Python-skriptet* exempel tillhandahålls också här.</span><span class="sxs-lookup"><span data-stu-id="55a26-117">*Bash script* and *Python script* examples are also provided here.</span></span> 

## <a name="powershell-script"></a><span data-ttu-id="55a26-118">PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="55a26-118">PowerShell script</span></span>
<span data-ttu-id="55a26-119">Det här skriptet visar hello skapandet av fyra användningsrapporter med hjälp av hello `TimeStamp` parametern och hello `ApplicationId` filter.</span><span class="sxs-lookup"><span data-stu-id="55a26-119">This script demonstrates hello creation of four usage reports by using hello `TimeStamp` parameter and hello `ApplicationId` filter.</span></span>

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


## <a name="usage-report-definitions"></a><span data-ttu-id="55a26-120">Användning rapportdefinitioner</span><span class="sxs-lookup"><span data-stu-id="55a26-120">Usage report definitions</span></span>
* <span data-ttu-id="55a26-121">**tenantUserCount**: hello antalet användare i hello innehavare av typ av identitetsleverantör per dag i hello senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="55a26-121">**tenantUserCount**: hello number of users in hello tenant by type of identity provider, per day in hello last 30 days.</span></span> <span data-ttu-id="55a26-122">(Du kan också en `TimeStamp` filtret innehåller användaren antal från ett visst datum toohello aktuellt datum).</span><span class="sxs-lookup"><span data-stu-id="55a26-122">(Optionally, a `TimeStamp` filter provides user counts from a specified date toohello current date).</span></span> <span data-ttu-id="55a26-123">hello rapporten innehåller:</span><span class="sxs-lookup"><span data-stu-id="55a26-123">hello report provides:</span></span>
  * <span data-ttu-id="55a26-124">**TotalUserCount**: hello antalet alla användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="55a26-124">**TotalUserCount**: hello number of all user objects.</span></span>
  * <span data-ttu-id="55a26-125">**OtherUserCount**: hello antalet Azure Active Directory-användare (inte Azure AD B2C-användare).</span><span class="sxs-lookup"><span data-stu-id="55a26-125">**OtherUserCount**: hello number of Azure Active Directory users (not Azure AD B2C users).</span></span>
  * <span data-ttu-id="55a26-126">**LocalUserCount**: hello antal Azure AD B2C-användarkonton som skapats med autentiseringsuppgifter för lokal toohello Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a26-126">**LocalUserCount**: hello number of Azure AD B2C user accounts created with credentials local toohello Azure AD B2C tenant.</span></span>

* <span data-ttu-id="55a26-127">**AlternateIdUserCount**: hello Azure AD B2C användare registrerats med externa identitetsleverantörer (till exempel Facebook, ett Microsoft-konto eller ett annat Azure Active Directory-klient, kallas även tooas en `OrgId`).</span><span class="sxs-lookup"><span data-stu-id="55a26-127">**AlternateIdUserCount**: hello number of Azure AD B2C users registered with external identity providers (for example, Facebook, a Microsoft account, or another Azure Active Directory tenant, also referred tooas an `OrgId`).</span></span>

* <span data-ttu-id="55a26-128">**b2cAuthenticationCountSummary**: Sammanfattning av hello dagliga antalet fakturerbar autentiseringar över hello senaste 30 dagarna, per dag och typ av autentiseringsflödet.</span><span class="sxs-lookup"><span data-stu-id="55a26-128">**b2cAuthenticationCountSummary**: Summary of hello daily number of billable authentications over hello last 30 days, by day and type of authentication flow.</span></span>

* <span data-ttu-id="55a26-129">**b2cAuthenticationCount**: hello antalet autentiseringar inom en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="55a26-129">**b2cAuthenticationCount**: hello number of authentications within a time period.</span></span> <span data-ttu-id="55a26-130">hello standardvärdet är hello senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="55a26-130">hello default is hello last 30 days.</span></span>  <span data-ttu-id="55a26-131">(Valfritt: hello inledande och avslutande `TimeStamp` parametrar som definierar en specifik tidsperiod.) hello utdata innehåller `StartTimeStamp` (tidigaste datumet för aktiviteten för den här klienten) och `EndTimeStamp` (senaste uppdatering).</span><span class="sxs-lookup"><span data-stu-id="55a26-131">(Optional: hello beginning and ending `TimeStamp` parameters define a specific time period.) hello output includes `StartTimeStamp` (earliest date of activity for this tenant) and `EndTimeStamp` (latest update).</span></span>

* <span data-ttu-id="55a26-132">**b2cMfaRequestCountSummary**: Sammanfattning av hello dagliga antalet Multi-Factor Authentication per dag och typ (SMS eller röst).</span><span class="sxs-lookup"><span data-stu-id="55a26-132">**b2cMfaRequestCountSummary**: Summary of hello daily number of multi-factor authentications, by day and type (SMS or voice).</span></span>


## <a name="limitations"></a><span data-ttu-id="55a26-133">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="55a26-133">Limitations</span></span>
<span data-ttu-id="55a26-134">Antal användardata uppdateras var 24 too48: e timme.</span><span class="sxs-lookup"><span data-stu-id="55a26-134">User count data is refreshed every 24 too48 hours.</span></span> <span data-ttu-id="55a26-135">Autentiseringar uppdateras flera gånger om dagen.</span><span class="sxs-lookup"><span data-stu-id="55a26-135">Authentications are updated several times a day.</span></span> <span data-ttu-id="55a26-136">När du använder hello `ApplicationId` filter, en tom rapport svaret kan vara på grund av tooone av följande villkor:</span><span class="sxs-lookup"><span data-stu-id="55a26-136">When using hello `ApplicationId` filter, an empty report response can be due tooone of following conditions:</span></span>
  * <span data-ttu-id="55a26-137">hello-ID: T finns inte i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="55a26-137">hello application ID does not exist in hello tenant.</span></span> <span data-ttu-id="55a26-138">Kontrollera att den är korrekt.</span><span class="sxs-lookup"><span data-stu-id="55a26-138">Make sure it is correct.</span></span>
  * <span data-ttu-id="55a26-139">hello-ID: T finns, men inga data hittades i hello rapporteringsperioden.</span><span class="sxs-lookup"><span data-stu-id="55a26-139">hello application ID exists, but no data was found in hello reporting period.</span></span> <span data-ttu-id="55a26-140">Granska parametrarna datum/tid.</span><span class="sxs-lookup"><span data-stu-id="55a26-140">Review your date/time parameters.</span></span>


## <a name="next-steps"></a><span data-ttu-id="55a26-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55a26-141">Next steps</span></span>
### <a name="monthly-bill-estimates-for-azure-ad"></a><span data-ttu-id="55a26-142">Månadsfaktura beräknar för Azure AD</span><span class="sxs-lookup"><span data-stu-id="55a26-142">Monthly bill estimates for Azure AD</span></span>
<span data-ttu-id="55a26-143">När den kombineras med [hello senaste Azure AD B2C priser tillgängliga](https://azure.microsoft.com/pricing/details/active-directory-b2c/), du kan beräkna dagliga, veckovisa och månatliga Azure-förbrukningen.</span><span class="sxs-lookup"><span data-stu-id="55a26-143">When combined with [hello most current Azure AD B2C pricing available](https://azure.microsoft.com/pricing/details/active-directory-b2c/), you can estimate daily, weekly, and monthly Azure consumption.</span></span>  <span data-ttu-id="55a26-144">En uppskattning är särskilt användbart när du planerar för ändringar i klient-funktioner som kan påverka övergripande kostnader.</span><span class="sxs-lookup"><span data-stu-id="55a26-144">An estimate is especially useful when you plan for changes in tenant behavior that might impact overall cost.</span></span> <span data-ttu-id="55a26-145">Du kan granska faktiska kostnader i din [länkade Azure-prenumeration](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="55a26-145">You can review actual costs in your [linked Azure subscription](active-directory-b2c-how-to-enable-billing.md).</span></span>

### <a name="options-for-other-output-formats"></a><span data-ttu-id="55a26-146">Alternativ för andra utdataformat</span><span class="sxs-lookup"><span data-stu-id="55a26-146">Options for other output formats</span></span>
<span data-ttu-id="55a26-147">hello visar följande kod exempel för att skicka utdata tooJSON, en lista med namn på värden och XML:</span><span class="sxs-lookup"><span data-stu-id="55a26-147">hello following code shows examples of sending output tooJSON, a name value list, and XML:</span></span>
```powershell
# toooutput tooJSON use following line in hello PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# toooutput hello content tooa name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# toooutput hello content in XML use hello following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
