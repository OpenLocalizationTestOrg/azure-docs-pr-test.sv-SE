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
ms.openlocfilehash: 52180b760046d273c5a75720df0a73532d0343ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a><span data-ttu-id="429f2-103">Åtkomst till användningsrapporter i Azure AD B2C via reporting API</span><span class="sxs-lookup"><span data-stu-id="429f2-103">Accessing usage reports in Azure AD B2C via the reporting API</span></span>

<span data-ttu-id="429f2-104">Azure Active Directory B2C (Azure AD B2C) ger autentisering baserat på användarens inloggning och Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="429f2-104">Azure Active Directory B2C (Azure AD B2C) provides authentication based on user sign-in and Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="429f2-105">Autentisering har angetts för slutanvändare i din familj program över identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="429f2-105">Authentication is provided for end users of your application family across identity providers.</span></span> <span data-ttu-id="429f2-106">När du vet hur många användare som har registrerats i innehavaren, providers som används för att registrera och antalet autentiseringar med typen kan du svara på frågor som:</span><span class="sxs-lookup"><span data-stu-id="429f2-106">When you know the number of users registered in the tenant, the providers they used to register, and the number of authentications by type, you can answer questions like:</span></span>
* <span data-ttu-id="429f2-107">Hur många användare från varje typ av identitetsleverantör (till exempel en Microsoft eller LinkedIn-konto) har registrerats under de senaste 10 dagarna?</span><span class="sxs-lookup"><span data-stu-id="429f2-107">How many users from each type of identity provider (for example, a Microsoft or LinkedIn account) have registered in the last 10 days?</span></span>
* <span data-ttu-id="429f2-108">Hur många autentiseringar med Multi-Factor Authentication har slutförts i den senaste månaden?</span><span class="sxs-lookup"><span data-stu-id="429f2-108">How many authentications using Multi-Factor Authentication have completed successfully in the last month?</span></span>
* <span data-ttu-id="429f2-109">Hur många tecken i baserade autentiseringar har slutfört den här månaden?</span><span class="sxs-lookup"><span data-stu-id="429f2-109">How many sign-in-based authentications were completed this month?</span></span> <span data-ttu-id="429f2-110">Per dag?</span><span class="sxs-lookup"><span data-stu-id="429f2-110">Per day?</span></span> <span data-ttu-id="429f2-111">Per program?</span><span class="sxs-lookup"><span data-stu-id="429f2-111">Per application?</span></span>
* <span data-ttu-id="429f2-112">Hur kan jag för att beräkna den förväntade månatliga kostnaden för aktiviteten min Azure AD B2C-klient?</span><span class="sxs-lookup"><span data-stu-id="429f2-112">How can I estimate the expected monthly cost of my Azure AD B2C tenant activity?</span></span>

<span data-ttu-id="429f2-113">Den här artikeln fokuserar på rapporter som är knutna till fakturering aktivitet som baseras på antalet användare, fakturerbar logga-baserat autentiseringar och Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="429f2-113">This article focuses on reports tied to billing activity, which is based on the number of users, billable sign-in-based authentications, and multi-factor authentications.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="429f2-114">Krav</span><span class="sxs-lookup"><span data-stu-id="429f2-114">Prerequisites</span></span>
<span data-ttu-id="429f2-115">Innan du börjar måste du slutföra stegen i [krav för att få åtkomst till Azure AD reporting API: er](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="429f2-115">Before you get started, you need to complete the steps in [Prerequisites to access the Azure AD reporting APIs](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span></span> <span data-ttu-id="429f2-116">Skapa ett program, skaffa en hemlighet för den och ge det åtkomst rättigheter till din Azure AD B2C-klient-rapporter.</span><span class="sxs-lookup"><span data-stu-id="429f2-116">Create an application, obtain a secret for it, and grant it access rights to your Azure AD B2C tenant’s reports.</span></span> <span data-ttu-id="429f2-117">*Bash skriptet* och *Python-skriptet* exempel tillhandahålls också här.</span><span class="sxs-lookup"><span data-stu-id="429f2-117">*Bash script* and *Python script* examples are also provided here.</span></span> 

## <a name="powershell-script"></a><span data-ttu-id="429f2-118">PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="429f2-118">PowerShell script</span></span>
<span data-ttu-id="429f2-119">Det här skriptet visar skapandet av fyra användningsrapporter med hjälp av den `TimeStamp` parameter och `ApplicationId` filter.</span><span class="sxs-lookup"><span data-stu-id="429f2-119">This script demonstrates the creation of four usage reports by using the `TimeStamp` parameter and the `ApplicationId` filter.</span></span>

```powershell
# This script will require the Web Application and permissions setup in Azure Active Directory

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

    Write-host Data from the tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for the report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for the " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a><span data-ttu-id="429f2-120">Användning rapportdefinitioner</span><span class="sxs-lookup"><span data-stu-id="429f2-120">Usage report definitions</span></span>
* <span data-ttu-id="429f2-121">**tenantUserCount**: antalet användare i innehavaren av typ av identitetsleverantör per dag under de senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="429f2-121">**tenantUserCount**: The number of users in the tenant by type of identity provider, per day in the last 30 days.</span></span> <span data-ttu-id="429f2-122">(Du kan också en `TimeStamp` filtret innehåller användaren antal från det angivna datumet till dagens datum).</span><span class="sxs-lookup"><span data-stu-id="429f2-122">(Optionally, a `TimeStamp` filter provides user counts from a specified date to the current date).</span></span> <span data-ttu-id="429f2-123">Den här rapporten innehåller:</span><span class="sxs-lookup"><span data-stu-id="429f2-123">The report provides:</span></span>
  * <span data-ttu-id="429f2-124">**TotalUserCount**: antalet alla användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="429f2-124">**TotalUserCount**: The number of all user objects.</span></span>
  * <span data-ttu-id="429f2-125">**OtherUserCount**: antalet Azure Active Directory-användare (inte Azure AD B2C-användare).</span><span class="sxs-lookup"><span data-stu-id="429f2-125">**OtherUserCount**: The number of Azure Active Directory users (not Azure AD B2C users).</span></span>
  * <span data-ttu-id="429f2-126">**LocalUserCount**: antal användarkonton för Azure AD B2C lokala till Azure AD B2C-klient skapas med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="429f2-126">**LocalUserCount**: The number of Azure AD B2C user accounts created with credentials local to the Azure AD B2C tenant.</span></span>

* <span data-ttu-id="429f2-127">**AlternateIdUserCount**: antalet Azure AD B2C-användare som har registrerats med externa identitetsleverantörer (till exempel Facebook, ett Microsoft-konto eller ett annat Azure Active Directory-klient, kallas även en `OrgId`).</span><span class="sxs-lookup"><span data-stu-id="429f2-127">**AlternateIdUserCount**: The number of Azure AD B2C users registered with external identity providers (for example, Facebook, a Microsoft account, or another Azure Active Directory tenant, also referred to as an `OrgId`).</span></span>

* <span data-ttu-id="429f2-128">**b2cAuthenticationCountSummary**: Sammanfattning av dagliga antalet fakturerbar autentiseringar under de senaste 30 dagarna, per dag och typ av autentiseringsflödet.</span><span class="sxs-lookup"><span data-stu-id="429f2-128">**b2cAuthenticationCountSummary**: Summary of the daily number of billable authentications over the last 30 days, by day and type of authentication flow.</span></span>

* <span data-ttu-id="429f2-129">**b2cAuthenticationCount**: Antal autentiseringar inom en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="429f2-129">**b2cAuthenticationCount**: The number of authentications within a time period.</span></span> <span data-ttu-id="429f2-130">Standardvärdet är de senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="429f2-130">The default is the last 30 days.</span></span>  <span data-ttu-id="429f2-131">(Valfritt: början och slutet `TimeStamp` parametrar som definierar en viss tidsperiod.) Utdata innehåller `StartTimeStamp` (tidigaste datumet för aktiviteten för den här klienten) och `EndTimeStamp` (senaste uppdatering).</span><span class="sxs-lookup"><span data-stu-id="429f2-131">(Optional: The beginning and ending `TimeStamp` parameters define a specific time period.) The output includes `StartTimeStamp` (earliest date of activity for this tenant) and `EndTimeStamp` (latest update).</span></span>

* <span data-ttu-id="429f2-132">**b2cMfaRequestCountSummary**: Sammanfattning av dagliga antalet Multi-Factor Authentication per dag och typ (SMS eller röst).</span><span class="sxs-lookup"><span data-stu-id="429f2-132">**b2cMfaRequestCountSummary**: Summary of the daily number of multi-factor authentications, by day and type (SMS or voice).</span></span>


## <a name="limitations"></a><span data-ttu-id="429f2-133">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="429f2-133">Limitations</span></span>
<span data-ttu-id="429f2-134">Antal användardata uppdateras var 24 till 48 timmar.</span><span class="sxs-lookup"><span data-stu-id="429f2-134">User count data is refreshed every 24 to 48 hours.</span></span> <span data-ttu-id="429f2-135">Autentiseringar uppdateras flera gånger om dagen.</span><span class="sxs-lookup"><span data-stu-id="429f2-135">Authentications are updated several times a day.</span></span> <span data-ttu-id="429f2-136">När du använder den `ApplicationId` filter, ett tomt rapporten svar kan bero på något av följande villkor:</span><span class="sxs-lookup"><span data-stu-id="429f2-136">When using the `ApplicationId` filter, an empty report response can be due to one of following conditions:</span></span>
  * <span data-ttu-id="429f2-137">Program-ID finns inte i klientorganisationen.</span><span class="sxs-lookup"><span data-stu-id="429f2-137">The application ID does not exist in the tenant.</span></span> <span data-ttu-id="429f2-138">Kontrollera att den är korrekt.</span><span class="sxs-lookup"><span data-stu-id="429f2-138">Make sure it is correct.</span></span>
  * <span data-ttu-id="429f2-139">Program-ID finns, men inga data hittades under rapporteringsperioden.</span><span class="sxs-lookup"><span data-stu-id="429f2-139">The application ID exists, but no data was found in the reporting period.</span></span> <span data-ttu-id="429f2-140">Granska parametrarna datum/tid.</span><span class="sxs-lookup"><span data-stu-id="429f2-140">Review your date/time parameters.</span></span>


## <a name="next-steps"></a><span data-ttu-id="429f2-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="429f2-141">Next steps</span></span>
### <a name="monthly-bill-estimates-for-azure-ad"></a><span data-ttu-id="429f2-142">Månadsfaktura beräknar för Azure AD</span><span class="sxs-lookup"><span data-stu-id="429f2-142">Monthly bill estimates for Azure AD</span></span>
<span data-ttu-id="429f2-143">När den kombineras med [senaste Azure AD B2C prissättning tillgängliga](https://azure.microsoft.com/pricing/details/active-directory-b2c/), du kan beräkna dagliga, veckovisa och månatliga Azure-förbrukningen.</span><span class="sxs-lookup"><span data-stu-id="429f2-143">When combined with [the most current Azure AD B2C pricing available](https://azure.microsoft.com/pricing/details/active-directory-b2c/), you can estimate daily, weekly, and monthly Azure consumption.</span></span>  <span data-ttu-id="429f2-144">En uppskattning är särskilt användbart när du planerar för ändringar i klient-funktioner som kan påverka övergripande kostnader.</span><span class="sxs-lookup"><span data-stu-id="429f2-144">An estimate is especially useful when you plan for changes in tenant behavior that might impact overall cost.</span></span> <span data-ttu-id="429f2-145">Du kan granska faktiska kostnader i din [länkade Azure-prenumeration](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="429f2-145">You can review actual costs in your [linked Azure subscription](active-directory-b2c-how-to-enable-billing.md).</span></span>

### <a name="options-for-other-output-formats"></a><span data-ttu-id="429f2-146">Alternativ för andra utdataformat</span><span class="sxs-lookup"><span data-stu-id="429f2-146">Options for other output formats</span></span>
<span data-ttu-id="429f2-147">Följande kod visar exempel för att skicka utdata till JSON, en lista med namn på värden och XML:</span><span class="sxs-lookup"><span data-stu-id="429f2-147">The following code shows examples of sending output to JSON, a name value list, and XML:</span></span>
```powershell
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
