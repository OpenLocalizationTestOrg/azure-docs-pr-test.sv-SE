---
title: aaaAzure Active Directory-inloggning aktivitetsrapport API-referens | Microsoft Docs
description: "Referens för hello Azure Active Directory-inloggning aktivitetsrapport API"
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
ms.openlocfilehash: 8123f308a291503f2c61ac5de26696806c6402ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a><span data-ttu-id="e81f1-103">Azure Active Directory inloggningsaktivitet rapporten API-referens</span><span class="sxs-lookup"><span data-stu-id="e81f1-103">Azure Active Directory sign-in activity report API reference</span></span>
<span data-ttu-id="e81f1-104">Det här avsnittet är en del av en samling ämnen om hello Azure Active Directory reporting API.</span><span class="sxs-lookup"><span data-stu-id="e81f1-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="e81f1-105">Azure AD-rapportering ger dig en API som gör att du tooaccess inloggningsaktivitet rapportdata via kod eller relaterade verktyg.</span><span class="sxs-lookup"><span data-stu-id="e81f1-105">Azure AD reporting provides you with an API that enables you tooaccess sign-in activity report data using code or related tools.</span></span>
<span data-ttu-id="e81f1-106">hello omfånget för det här avsnittet är tooprovide dig med information om hello **aktivitet rapporten API inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e81f1-106">hello scope of this topic is tooprovide you with reference information about hello **sign-in activity report API**.</span></span>

<span data-ttu-id="e81f1-107">Se:</span><span class="sxs-lookup"><span data-stu-id="e81f1-107">See:</span></span>

* <span data-ttu-id="e81f1-108">[Logga in aktiviteter](active-directory-reporting-azure-portal.md#activity-reports) mer information</span><span class="sxs-lookup"><span data-stu-id="e81f1-108">[Sign-in activities](active-directory-reporting-azure-portal.md#activity-reports) for more conceptual information</span></span>
* <span data-ttu-id="e81f1-109">[Komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) mer information om hello reporting API.</span><span class="sxs-lookup"><span data-stu-id="e81f1-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


## <a name="who-can-access-hello-api-data"></a><span data-ttu-id="e81f1-110">Vem som kan komma åt hello API data?</span><span class="sxs-lookup"><span data-stu-id="e81f1-110">Who can access hello API data?</span></span>
* <span data-ttu-id="e81f1-111">Användare och tjänstens huvudnamn i hello säkerhet Admin eller säkerhet Reader roll</span><span class="sxs-lookup"><span data-stu-id="e81f1-111">Users and Service Principals in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="e81f1-112">Globala administratörer</span><span class="sxs-lookup"><span data-stu-id="e81f1-112">Global Admins</span></span>
* <span data-ttu-id="e81f1-113">Alla appar som har tillståndet tooaccess hello API (auktorisering i apptjänst kan vara installationen endast baserat på Global administratör behörighet)</span><span class="sxs-lookup"><span data-stu-id="e81f1-113">Any app that has authorization tooaccess hello API (app authorization can be setup only based on Global Admin’s permission)</span></span>

<span data-ttu-id="e81f1-114">tooconfigure åtkomst för en tooaccess programsäkerhet API: er, till exempel logga händelser, Använd hello följande PowerShell tooadd hello program tjänstens huvudnamn i rollen för hello säkerhet läsare</span><span class="sxs-lookup"><span data-stu-id="e81f1-114">tooconfigure access for an application tooaccess security APIs such as signin events, use hello following PowerShell tooadd hello applications Service Principal into hello Security Reader role</span></span>

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a><span data-ttu-id="e81f1-115">Krav</span><span class="sxs-lookup"><span data-stu-id="e81f1-115">Prerequisites</span></span>
<span data-ttu-id="e81f1-116">tooaccess detta rapporterar via hello reporting API måste du ha:</span><span class="sxs-lookup"><span data-stu-id="e81f1-116">tooaccess this report through hello reporting API, you must have:</span></span>

* <span data-ttu-id="e81f1-117">En [Azure Active Directory Premium P1 eller P2 edition](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="e81f1-117">An [Azure Active Directory Premium P1 or P2 edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="e81f1-118">Slutförda hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="e81f1-118">Completed hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-hello-api"></a><span data-ttu-id="e81f1-119">Åtkomst till hello-API</span><span class="sxs-lookup"><span data-stu-id="e81f1-119">Accessing hello API</span></span>
<span data-ttu-id="e81f1-120">Du kan antingen använda detta API via hello [diagram Explorer](https://graphexplorer2.cloudapp.net) eller programmässigt med, till exempel PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e81f1-120">You can either access this API through hello [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="e81f1-121">I ordning för PowerShell toocorrectly tolka hello OData filter syntax används i AAD diagram REST-anrop, måste du använda hello backtick (även kallat: allvarligt accent) tecken för ”undantagstecken” hello $.</span><span class="sxs-lookup"><span data-stu-id="e81f1-121">In order for PowerShell toocorrectly interpret hello OData filter syntax used in AAD Graph REST calls, you must use hello backtick (aka: grave accent) character too“escape” hello $ character.</span></span> <span data-ttu-id="e81f1-122">Hej backtick tecknet som fungerar som [PowerShells escape-tecknet](https://technet.microsoft.com/library/hh847755.aspx), vilket gör att PowerShell toodo en literal tolkning av hello $-tecken och undvika förvirrande som ett PowerShell-variabelnamn (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="e81f1-122">hello backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell toodo a literal interpretation of hello $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="e81f1-123">hello fokuserar i det här avsnittet på hello diagrammet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e81f1-123">hello focus of this topic is on hello Graph Explorer.</span></span> <span data-ttu-id="e81f1-124">Ett PowerShell-exempel finns [PowerShell-skriptet](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="e81f1-124">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="e81f1-125">API-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e81f1-125">API Endpoint</span></span>
<span data-ttu-id="e81f1-126">Du kan komma åt den här API: et med hello följande bas-URI:</span><span class="sxs-lookup"><span data-stu-id="e81f1-126">You can access this API using hello following base URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



<span data-ttu-id="e81f1-127">Detta API är begränsad till en miljon returnerade poster på grund av toohello mängden data.</span><span class="sxs-lookup"><span data-stu-id="e81f1-127">Due toohello volume of data, this API has a limit of one million returned records.</span></span> 

<span data-ttu-id="e81f1-128">Det här anropet returnerar hello data i batchar.</span><span class="sxs-lookup"><span data-stu-id="e81f1-128">This call returns hello data in batches.</span></span> <span data-ttu-id="e81f1-129">Varje grupp som har högst 1000 poster.</span><span class="sxs-lookup"><span data-stu-id="e81f1-129">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="e81f1-130">tooget hello nästa batch poster, Använd hello nästa länk.</span><span class="sxs-lookup"><span data-stu-id="e81f1-130">tooget hello next batch of records, use hello Next link.</span></span> <span data-ttu-id="e81f1-131">Hämta hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information från hello första uppsättning returnerade poster.</span><span class="sxs-lookup"><span data-stu-id="e81f1-131">Get hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information from hello first set of returned records.</span></span> <span data-ttu-id="e81f1-132">hello skip-token kommer att hello slutet av hello resultatmängden.</span><span class="sxs-lookup"><span data-stu-id="e81f1-132">hello skip token will be at hello end of hello result set.</span></span>  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a><span data-ttu-id="e81f1-133">Filter som stöds</span><span class="sxs-lookup"><span data-stu-id="e81f1-133">Supported filters</span></span>
<span data-ttu-id="e81f1-134">Du kan begränsa hello antalet poster som returneras av en API-anrop i form av ett filter.</span><span class="sxs-lookup"><span data-stu-id="e81f1-134">You can narrow down hello number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="e81f1-135">Relaterade data, hello följande filter stöds för inloggning API:</span><span class="sxs-lookup"><span data-stu-id="e81f1-135">For sign-in API related data, hello following filters are supported:</span></span>

* <span data-ttu-id="e81f1-136">**$top =\<antal poster toobe returneras\>**  -toolimit hello antalet returnerade poster.</span><span class="sxs-lookup"><span data-stu-id="e81f1-136">**$top=\<number of records toobe returned\>** - toolimit hello number of returned records.</span></span> <span data-ttu-id="e81f1-137">Det här är en kostsam åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e81f1-137">This is an expensive operation.</span></span> <span data-ttu-id="e81f1-138">Du bör inte använda det här filtret om du vill tooreturn tusentals objekt.</span><span class="sxs-lookup"><span data-stu-id="e81f1-138">You should not use this filter if you want tooreturn thousands of objects.</span></span>  
* <span data-ttu-id="e81f1-139">**$filter =\<filter-instruktionen\>**  -toospecify hello baserat på filter som stöds fält, hello typ av poster som intresserar dig</span><span class="sxs-lookup"><span data-stu-id="e81f1-139">**$filter=\<your filter statement\>** - toospecify, on hello basis of supported filter fields, hello type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="e81f1-140">Filter som stöds fält och operatorer</span><span class="sxs-lookup"><span data-stu-id="e81f1-140">Supported filter fields and operators</span></span>
<span data-ttu-id="e81f1-141">toospecify hello typ av poster som intresserar dig, kan du skapa ett filter-uttryck som kan innehålla en eller en kombination av hello följande filterfält:</span><span class="sxs-lookup"><span data-stu-id="e81f1-141">toospecify hello type of records you care about, you can build a filter statement that can contain either one or a combination of hello following filter fields:</span></span>

* <span data-ttu-id="e81f1-142">[signinDateTime](#signindatetime) -definierar ett datum eller datumintervall</span><span class="sxs-lookup"><span data-stu-id="e81f1-142">[signinDateTime](#signindatetime) - defines a date or date range</span></span>
* <span data-ttu-id="e81f1-143">[användar-ID](#userid) -definierar en specifik användare baserade hello användar-ID.</span><span class="sxs-lookup"><span data-stu-id="e81f1-143">[userId](#userid) - defines a specific user based hello user's ID.</span></span>
* <span data-ttu-id="e81f1-144">[userPrincipalName](#userprincipalname) -definierar en specifik användare baserade hello användarens huvudnamn (UPN)</span><span class="sxs-lookup"><span data-stu-id="e81f1-144">[userPrincipalName](#userprincipalname) - defines a specific user based hello user's user principal name (UPN)</span></span>
* <span data-ttu-id="e81f1-145">[appId](#appid) -definierar en viss app baserat hello app-ID</span><span class="sxs-lookup"><span data-stu-id="e81f1-145">[appId](#appid) - defines a specific app based hello app's ID</span></span>
* <span data-ttu-id="e81f1-146">[appDisplayName](#appdisplayname) -definierar en viss app baserat hello app visningsnamn</span><span class="sxs-lookup"><span data-stu-id="e81f1-146">[appDisplayName](#appdisplayname) - defines a specific app based hello app's display name</span></span>
* <span data-ttu-id="e81f1-147">[loginStatus](#loginStatus) -definierar hello status för hello inloggningar (lyckade / misslyckade)</span><span class="sxs-lookup"><span data-stu-id="e81f1-147">[loginStatus](#loginStatus) - defines hello status of hello logins (success / failure)</span></span>

> [!NOTE]
> <span data-ttu-id="e81f1-148">När du använder diagram Explorer, måste du hello toouse hello rätt fallet för varje brev är filterfält.</span><span class="sxs-lookup"><span data-stu-id="e81f1-148">When using Graph Explorer, you hello need toouse hello correct case for each letter is your filter fields.</span></span>
> 
> 

<span data-ttu-id="e81f1-149">Du kan skapa kombinationer av hello stöds filter och filterfält toonarrow ned hello omfattning hello returnerade data.</span><span class="sxs-lookup"><span data-stu-id="e81f1-149">toonarrow down hello scope of hello returned data, you can build combinations of hello supported filters and filter fields.</span></span> <span data-ttu-id="e81f1-150">Till exempel returnerar hello följande sats hello översta 10 poster mellan 1 juli 2016 och juli 6 2016:</span><span class="sxs-lookup"><span data-stu-id="e81f1-150">For example, hello following statement returns hello top 10 records between July 1st 2016 and July 6th 2016:</span></span>

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a><span data-ttu-id="e81f1-151">signinDateTime</span><span class="sxs-lookup"><span data-stu-id="e81f1-151">signinDateTime</span></span>
<span data-ttu-id="e81f1-152">**Stöd för operatörer**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="e81f1-152">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="e81f1-153">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-153">**Example**:</span></span>

<span data-ttu-id="e81f1-154">Med hjälp av ett visst datum</span><span class="sxs-lookup"><span data-stu-id="e81f1-154">Using a specific date</span></span>

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



<span data-ttu-id="e81f1-155">Med hjälp av ett datumintervall</span><span class="sxs-lookup"><span data-stu-id="e81f1-155">Using a date range</span></span>    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


<span data-ttu-id="e81f1-156">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-156">**Notes**:</span></span>

<span data-ttu-id="e81f1-157">hello datetime-parametern måste vara i hello UTC-format</span><span class="sxs-lookup"><span data-stu-id="e81f1-157">hello datetime parameter should be in hello UTC format</span></span> 

- - -
### <a name="userid"></a><span data-ttu-id="e81f1-158">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="e81f1-158">userId</span></span>
<span data-ttu-id="e81f1-159">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e81f1-159">**Supported operators**: eq</span></span>

<span data-ttu-id="e81f1-160">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-160">**Example**:</span></span>

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

<span data-ttu-id="e81f1-161">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-161">**Notes**:</span></span>

<span data-ttu-id="e81f1-162">hello-värdet för användar-ID är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="e81f1-162">hello value of userId is a string value</span></span>

- - -
### <a name="userprincipalname"></a><span data-ttu-id="e81f1-163">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="e81f1-163">userPrincipalName</span></span>
<span data-ttu-id="e81f1-164">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e81f1-164">**Supported operators**: eq</span></span>

<span data-ttu-id="e81f1-165">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-165">**Example**:</span></span>

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


<span data-ttu-id="e81f1-166">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-166">**Notes**:</span></span>

<span data-ttu-id="e81f1-167">hello-värdet för userPrincipalName är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="e81f1-167">hello value of userPrincipalName is a string value</span></span>

- - -
### <a name="appid"></a><span data-ttu-id="e81f1-168">appId</span><span class="sxs-lookup"><span data-stu-id="e81f1-168">appId</span></span>
<span data-ttu-id="e81f1-169">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e81f1-169">**Supported operators**: eq</span></span>

<span data-ttu-id="e81f1-170">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-170">**Example**:</span></span>

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



<span data-ttu-id="e81f1-171">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-171">**Notes**:</span></span>

<span data-ttu-id="e81f1-172">hello-värdet för appId är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="e81f1-172">hello value of appId is a string value</span></span>

- - -
### <a name="appdisplayname"></a><span data-ttu-id="e81f1-173">appDisplayName</span><span class="sxs-lookup"><span data-stu-id="e81f1-173">appDisplayName</span></span>
<span data-ttu-id="e81f1-174">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e81f1-174">**Supported operators**: eq</span></span>

<span data-ttu-id="e81f1-175">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-175">**Example**:</span></span>

    $filter=appDisplayName+eq+'Azure+Portal' 


<span data-ttu-id="e81f1-176">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-176">**Notes**:</span></span>

<span data-ttu-id="e81f1-177">hello-värdet för appDisplayName är ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="e81f1-177">hello value of appDisplayName is a string value</span></span>

- - -
### <a name="loginstatus"></a><span data-ttu-id="e81f1-178">loginStatus</span><span class="sxs-lookup"><span data-stu-id="e81f1-178">loginStatus</span></span>
<span data-ttu-id="e81f1-179">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e81f1-179">**Supported operators**: eq</span></span>

<span data-ttu-id="e81f1-180">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-180">**Example**:</span></span>

    $filter=loginStatus+eq+'1'  


<span data-ttu-id="e81f1-181">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e81f1-181">**Notes**:</span></span>

<span data-ttu-id="e81f1-182">Det finns två alternativ för hello loginStatus: 0 - lyckades, 1 – fel</span><span class="sxs-lookup"><span data-stu-id="e81f1-182">There are two options for hello loginStatus: 0 - success, 1 - failure</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="e81f1-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e81f1-183">Next steps</span></span>
* <span data-ttu-id="e81f1-184">Vill du toosee exempel för filtrerade inloggning aktiviteter?</span><span class="sxs-lookup"><span data-stu-id="e81f1-184">Do you want toosee examples for filtered sign-in activities?</span></span> <span data-ttu-id="e81f1-185">Kolla in hello [Azure Active Directory inloggningsaktivitet rapporten API-exempel](active-directory-reporting-api-sign-in-activity-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e81f1-185">Check out hello [Azure Active Directory sign-in activity report API samples](active-directory-reporting-api-sign-in-activity-samples.md).</span></span>
* <span data-ttu-id="e81f1-186">Vill du tooknow mer om hello Azure AD reporting API?</span><span class="sxs-lookup"><span data-stu-id="e81f1-186">Do you want tooknow more about hello Azure AD reporting API?</span></span> <span data-ttu-id="e81f1-187">Se [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="e81f1-187">See [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

