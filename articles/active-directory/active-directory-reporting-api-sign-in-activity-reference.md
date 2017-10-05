---
title: Azure Active Directory inloggningsaktivitet rapporten API-referens | Microsoft Docs
description: "Referens för Azure Active Directory inloggningsaktivitet rapporten API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d83f1a899ba38dab2c1c1661adede87db6f88c20
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a><span data-ttu-id="ae989-103">Azure Active Directory inloggningsaktivitet rapporten API-referens</span><span class="sxs-lookup"><span data-stu-id="ae989-103">Azure Active Directory sign-in activity report API reference</span></span>
<span data-ttu-id="ae989-104">Det här avsnittet är en del av en samling ämnen om Azure Active Directory reporting API.</span><span class="sxs-lookup"><span data-stu-id="ae989-104">This topic is part of a collection of topics about the Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="ae989-105">Azure AD-rapportering ger dig en API som gör att du kan komma åt inloggningsaktivitet rapportdata via kod eller relaterade verktyg.</span><span class="sxs-lookup"><span data-stu-id="ae989-105">Azure AD reporting provides you with an API that enables you to access sign-in activity report data using code or related tools.</span></span>
<span data-ttu-id="ae989-106">Omfånget för det här avsnittet är att ge dig referensinformation om den **aktivitet rapporten API inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ae989-106">The scope of this topic is to provide you with reference information about the **sign-in activity report API**.</span></span>

<span data-ttu-id="ae989-107">Se:</span><span class="sxs-lookup"><span data-stu-id="ae989-107">See:</span></span>

* <span data-ttu-id="ae989-108">[Logga in aktiviteter](active-directory-reporting-azure-portal.md#activity-reports) mer information</span><span class="sxs-lookup"><span data-stu-id="ae989-108">[Sign-in activities](active-directory-reporting-azure-portal.md#activity-reports) for more conceptual information</span></span>
* <span data-ttu-id="ae989-109">[Komma igång med Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) för mer information om reporting API.</span><span class="sxs-lookup"><span data-stu-id="ae989-109">[Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about the reporting API.</span></span>


## <a name="who-can-access-the-api-data"></a><span data-ttu-id="ae989-110">Vem som har åtkomst till API-data?</span><span class="sxs-lookup"><span data-stu-id="ae989-110">Who can access the API data?</span></span>
* <span data-ttu-id="ae989-111">Användare och tjänstens huvudnamn i rollen Administratör säkerhet eller säkerhet läsare</span><span class="sxs-lookup"><span data-stu-id="ae989-111">Users and Service Principals in the Security Admin or Security Reader role</span></span>
* <span data-ttu-id="ae989-112">Globala administratörer</span><span class="sxs-lookup"><span data-stu-id="ae989-112">Global Admins</span></span>
* <span data-ttu-id="ae989-113">Alla appar som har behörighet att komma åt API: et (auktorisering i apptjänst kan vara installationen endast baserat på Global administratör behörighet)</span><span class="sxs-lookup"><span data-stu-id="ae989-113">Any app that has authorization to access the API (app authorization can be setup only based on Global Admin’s permission)</span></span>

<span data-ttu-id="ae989-114">Använd följande PowerShell att lägga till program tjänstens huvudnamn i rollen Läsare av säkerhet för att konfigurera åtkomst för ett program för att komma åt säkerheten API: er, till exempel logga händelser</span><span class="sxs-lookup"><span data-stu-id="ae989-114">To configure access for an application to access security APIs such as signin events, use the following PowerShell to add the applications Service Principal into the Security Reader role</span></span>

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a><span data-ttu-id="ae989-115">Krav</span><span class="sxs-lookup"><span data-stu-id="ae989-115">Prerequisites</span></span>
<span data-ttu-id="ae989-116">Du måste ha för att komma åt den här rapporten via reporting API:</span><span class="sxs-lookup"><span data-stu-id="ae989-116">To access this report through the reporting API, you must have:</span></span>

* <span data-ttu-id="ae989-117">En [Azure Active Directory Premium P1 eller P2 edition](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="ae989-117">An [Azure Active Directory Premium P1 or P2 edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="ae989-118">Slutfört den [krav för att få åtkomst till Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="ae989-118">Completed the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-the-api"></a><span data-ttu-id="ae989-119">Åtkomst till API: et</span><span class="sxs-lookup"><span data-stu-id="ae989-119">Accessing the API</span></span>
<span data-ttu-id="ae989-120">Du kan antingen komma åt den här API via den [diagram Explorer](https://graphexplorer2.cloudapp.net) eller programmässigt med, till exempel PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae989-120">You can either access this API through the [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="ae989-121">För PowerShell för att tolka OData-filtersyntaxen som används i AAD diagram REST-anrop, måste du använda backtick (även kallat: allvarligt accent) tecken för att ”avbryta” $-tecken.</span><span class="sxs-lookup"><span data-stu-id="ae989-121">In order for PowerShell to correctly interpret the OData filter syntax used in AAD Graph REST calls, you must use the backtick (aka: grave accent) character to “escape” the $ character.</span></span> <span data-ttu-id="ae989-122">Tecknet backtick fungerar som [PowerShells escape-tecknet](https://technet.microsoft.com/library/hh847755.aspx), vilket gör att PowerShell för att göra en literal tolkning av $-tecken, och undvika förvirrande som ett PowerShell-variabelnamn (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="ae989-122">The backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell to do a literal interpretation of the $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="ae989-123">Det här avsnittet fokuserar på diagrammet Explorer.</span><span class="sxs-lookup"><span data-stu-id="ae989-123">The focus of this topic is on the Graph Explorer.</span></span> <span data-ttu-id="ae989-124">Ett PowerShell-exempel finns [PowerShell-skriptet](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="ae989-124">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="ae989-125">API-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="ae989-125">API Endpoint</span></span>
<span data-ttu-id="ae989-126">Du kan komma åt den här API: et med följande bas-URI:</span><span class="sxs-lookup"><span data-stu-id="ae989-126">You can access this API using the following base URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



<span data-ttu-id="ae989-127">Detta API är begränsad till en miljon returnerade poster på grund av mängden data.</span><span class="sxs-lookup"><span data-stu-id="ae989-127">Due to the volume of data, this API has a limit of one million returned records.</span></span> 

<span data-ttu-id="ae989-128">Det här anropet returnerar data i batchar.</span><span class="sxs-lookup"><span data-stu-id="ae989-128">This call returns the data in batches.</span></span> <span data-ttu-id="ae989-129">Varje grupp som har högst 1000 poster.</span><span class="sxs-lookup"><span data-stu-id="ae989-129">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="ae989-130">Använd nästa länken för att få poster nästa batch.</span><span class="sxs-lookup"><span data-stu-id="ae989-130">To get the next batch of records, use the Next link.</span></span> <span data-ttu-id="ae989-131">Hämta den [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information från den första uppsättningen returnerade poster.</span><span class="sxs-lookup"><span data-stu-id="ae989-131">Get the [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information from the first set of returned records.</span></span> <span data-ttu-id="ae989-132">Skip-token anges i slutet av resultatet.</span><span class="sxs-lookup"><span data-stu-id="ae989-132">The skip token will be at the end of the result set.</span></span>  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a><span data-ttu-id="ae989-133">Filter som stöds</span><span class="sxs-lookup"><span data-stu-id="ae989-133">Supported filters</span></span>
<span data-ttu-id="ae989-134">Du kan begränsa antalet poster som returneras av en API-anrop i form av ett filter.</span><span class="sxs-lookup"><span data-stu-id="ae989-134">You can narrow down the number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="ae989-135">Relaterade data, för inloggning API följande filter som stöds:</span><span class="sxs-lookup"><span data-stu-id="ae989-135">For sign-in API related data, the following filters are supported:</span></span>

* <span data-ttu-id="ae989-136">**$top =\<antal poster som ska returneras\>**  - om du vill begränsa antalet returnerade poster.</span><span class="sxs-lookup"><span data-stu-id="ae989-136">**$top=\<number of records to be returned\>** - to limit the number of returned records.</span></span> <span data-ttu-id="ae989-137">Det här är en kostsam åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ae989-137">This is an expensive operation.</span></span> <span data-ttu-id="ae989-138">Du bör inte använda det här filtret om du vill returnera tusentals objekt.</span><span class="sxs-lookup"><span data-stu-id="ae989-138">You should not use this filter if you want to return thousands of objects.</span></span>  
* <span data-ttu-id="ae989-139">**$filter =\<filter-instruktionen\>**  - om du vill ange vilken typ av poster som intresserar dig på grundval av filter som stöds fält</span><span class="sxs-lookup"><span data-stu-id="ae989-139">**$filter=\<your filter statement\>** - to specify, on the basis of supported filter fields, the type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="ae989-140">Filter som stöds fält och operatorer</span><span class="sxs-lookup"><span data-stu-id="ae989-140">Supported filter fields and operators</span></span>
<span data-ttu-id="ae989-141">Du kan skapa ett filter-uttryck som kan innehålla ett eller flera av följande filterfält om du vill ange vilken typ av poster som du är intresserad:</span><span class="sxs-lookup"><span data-stu-id="ae989-141">To specify the type of records you care about, you can build a filter statement that can contain either one or a combination of the following filter fields:</span></span>

* <span data-ttu-id="ae989-142">[signinDateTime](#signindatetime) -definierar ett datum eller datumintervall</span><span class="sxs-lookup"><span data-stu-id="ae989-142">[signinDateTime](#signindatetime) - defines a date or date range</span></span>
* <span data-ttu-id="ae989-143">[användar-ID](#userid) -definierar en specifik användare baserat på användar-ID.</span><span class="sxs-lookup"><span data-stu-id="ae989-143">[userId](#userid) - defines a specific user based the user's ID.</span></span>
* <span data-ttu-id="ae989-144">[userPrincipalName](#userprincipalname) -definierar en specifik användare baserat på användarens huvudnamn (UPN)</span><span class="sxs-lookup"><span data-stu-id="ae989-144">[userPrincipalName](#userprincipalname) - defines a specific user based the user's user principal name (UPN)</span></span>
* <span data-ttu-id="ae989-145">[appId](#appid) -definierar en specifik app baserat på app-ID</span><span class="sxs-lookup"><span data-stu-id="ae989-145">[appId](#appid) - defines a specific app based the app's ID</span></span>
* <span data-ttu-id="ae989-146">[appDisplayName](#appdisplayname) -definierar en specifik app baserat appens namn</span><span class="sxs-lookup"><span data-stu-id="ae989-146">[appDisplayName](#appdisplayname) - defines a specific app based the app's display name</span></span>
* <span data-ttu-id="ae989-147">[loginStatus](#loginStatus) -definierar status för inloggningar (lyckade / misslyckade)</span><span class="sxs-lookup"><span data-stu-id="ae989-147">[loginStatus](#loginStatus) - defines the status of the logins (success / failure)</span></span>

> [!NOTE]
> <span data-ttu-id="ae989-148">När du använder diagram Explorer, är du behöver ha rätt skiftläge för varje bokstav filterfält.</span><span class="sxs-lookup"><span data-stu-id="ae989-148">When using Graph Explorer, you the need to use the correct case for each letter is your filter fields.</span></span>
> 
> 

<span data-ttu-id="ae989-149">Du kan skapa kombinationer av fälten stöds filter och filtrera om du vill begränsa omfånget för returnerade data.</span><span class="sxs-lookup"><span data-stu-id="ae989-149">To narrow down the scope of the returned data, you can build combinations of the supported filters and filter fields.</span></span> <span data-ttu-id="ae989-150">Till exempel returnerar följande sats de översta 10 posterna mellan 1 juli 2016 och juli 6 2016:</span><span class="sxs-lookup"><span data-stu-id="ae989-150">For example, the following statement returns the top 10 records between July 1st 2016 and July 6th 2016:</span></span>

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a><span data-ttu-id="ae989-151">signinDateTime</span><span class="sxs-lookup"><span data-stu-id="ae989-151">signinDateTime</span></span>
<span data-ttu-id="ae989-152">**Stöd för operatörer**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="ae989-152">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="ae989-153">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="ae989-153">**Example**:</span></span>

<span data-ttu-id="ae989-154">Med hjälp av ett visst datum</span><span class="sxs-lookup"><span data-stu-id="ae989-154">Using a specific date</span></span>

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



<span data-ttu-id="ae989-155">Med hjälp av ett datumintervall</span><span class="sxs-lookup"><span data-stu-id="ae989-155">Using a date range</span></span>    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


<span data-ttu-id="ae989-156">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="ae989-156">**Notes**:</span></span>

<span data-ttu-id="ae989-157">Datetime-parametern måste vara i UTC-format</span><span class="sxs-lookup"><span data-stu-id="ae989-157">The datetime parameter should be in the UTC format</span></span> 

- - -
### <a name="userid"></a><span data-ttu-id="ae989-158">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="ae989-158">userId</span></span>
<span data-ttu-id="ae989-159">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="ae989-159">**Supported operators**: eq</span></span>

<span data-ttu-id="ae989-160">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="ae989-160">**Example**:</span></span>

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

<span data-ttu-id="ae989-161">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="ae989-161">**Notes**:</span></span>

<span data-ttu-id="ae989-162">Värdet för användar-ID är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="ae989-162">The value of userId is a string value</span></span>

- - -
### <a name="userprincipalname"></a><span data-ttu-id="ae989-163">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="ae989-163">userPrincipalName</span></span>
<span data-ttu-id="ae989-164">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="ae989-164">**Supported operators**: eq</span></span>

<span data-ttu-id="ae989-165">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="ae989-165">**Example**:</span></span>

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


<span data-ttu-id="ae989-166">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="ae989-166">**Notes**:</span></span>

<span data-ttu-id="ae989-167">Värdet för userPrincipalName är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="ae989-167">The value of userPrincipalName is a string value</span></span>

- - -
### <a name="appid"></a><span data-ttu-id="ae989-168">appId</span><span class="sxs-lookup"><span data-stu-id="ae989-168">appId</span></span>
<span data-ttu-id="ae989-169">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="ae989-169">**Supported operators**: eq</span></span>

<span data-ttu-id="ae989-170">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="ae989-170">**Example**:</span></span>

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



<span data-ttu-id="ae989-171">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="ae989-171">**Notes**:</span></span>

<span data-ttu-id="ae989-172">Värdet för appId är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="ae989-172">The value of appId is a string value</span></span>

- - -
### <a name="appdisplayname"></a><span data-ttu-id="ae989-173">appDisplayName</span><span class="sxs-lookup"><span data-stu-id="ae989-173">appDisplayName</span></span>
<span data-ttu-id="ae989-174">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="ae989-174">**Supported operators**: eq</span></span>

<span data-ttu-id="ae989-175">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="ae989-175">**Example**:</span></span>

    $filter=appDisplayName+eq+'Azure+Portal' 


<span data-ttu-id="ae989-176">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="ae989-176">**Notes**:</span></span>

<span data-ttu-id="ae989-177">Värdet för appDisplayName är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="ae989-177">The value of appDisplayName is a string value</span></span>

- - -
### <a name="loginstatus"></a><span data-ttu-id="ae989-178">loginStatus</span><span class="sxs-lookup"><span data-stu-id="ae989-178">loginStatus</span></span>
<span data-ttu-id="ae989-179">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="ae989-179">**Supported operators**: eq</span></span>

<span data-ttu-id="ae989-180">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="ae989-180">**Example**:</span></span>

    $filter=loginStatus+eq+'1'  


<span data-ttu-id="ae989-181">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="ae989-181">**Notes**:</span></span>

<span data-ttu-id="ae989-182">Det finns två alternativ för loginStatus: 0 - lyckades, 1 – fel</span><span class="sxs-lookup"><span data-stu-id="ae989-182">There are two options for the loginStatus: 0 - success, 1 - failure</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="ae989-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae989-183">Next steps</span></span>
* <span data-ttu-id="ae989-184">Vill du se exempel för filtrerade inloggning aktiviteter?</span><span class="sxs-lookup"><span data-stu-id="ae989-184">Do you want to see examples for filtered sign-in activities?</span></span> <span data-ttu-id="ae989-185">Kolla in den [Azure Active Directory inloggningsaktivitet rapporten API-exempel](active-directory-reporting-api-sign-in-activity-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ae989-185">Check out the [Azure Active Directory sign-in activity report API samples](active-directory-reporting-api-sign-in-activity-samples.md).</span></span>
* <span data-ttu-id="ae989-186">Vill du veta mer om Azure AD reporting API?</span><span class="sxs-lookup"><span data-stu-id="ae989-186">Do you want to know more about the Azure AD reporting API?</span></span> <span data-ttu-id="ae989-187">Se [komma igång med Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ae989-187">See [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

