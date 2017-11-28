---
title: aaaAzure Active Directory audit API-referens | Microsoft Docs
description: "Hur tooget igång med hello Azure Active Directory granska API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5f33b62ede9be445f35704739e328580dc454368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-api-reference"></a><span data-ttu-id="e6bab-103">Azure Active Directory audit API-referens</span><span class="sxs-lookup"><span data-stu-id="e6bab-103">Azure Active Directory audit API reference</span></span>
<span data-ttu-id="e6bab-104">Det här avsnittet är en del av en samling ämnen om hello Azure Active Directory reporting API.</span><span class="sxs-lookup"><span data-stu-id="e6bab-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="e6bab-105">Azure AD-rapportering ger dig en API som gör att du tooaccess granskningsdata via kod eller relaterade verktyg.</span><span class="sxs-lookup"><span data-stu-id="e6bab-105">Azure AD reporting provides you with an API that enables you tooaccess audit data using code or related tools.</span></span>
<span data-ttu-id="e6bab-106">hello omfånget för det här avsnittet är tooprovide dig med information om hello **granska API**.</span><span class="sxs-lookup"><span data-stu-id="e6bab-106">hello scope of this topic is tooprovide you with reference information about hello **audit API**.</span></span>

<span data-ttu-id="e6bab-107">Se:</span><span class="sxs-lookup"><span data-stu-id="e6bab-107">See:</span></span>

* <span data-ttu-id="e6bab-108">[Granskningsloggar](active-directory-reporting-azure-portal.md#activity-reports) mer information</span><span class="sxs-lookup"><span data-stu-id="e6bab-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>

* <span data-ttu-id="e6bab-109">[Komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) mer information om hello reporting API.</span><span class="sxs-lookup"><span data-stu-id="e6bab-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


<span data-ttu-id="e6bab-110">För:</span><span class="sxs-lookup"><span data-stu-id="e6bab-110">For:</span></span>

- <span data-ttu-id="e6bab-111">Vanliga frågor och svar, Läs vår [vanliga frågor och svar](active-directory-reporting-faq.md)</span><span class="sxs-lookup"><span data-stu-id="e6bab-111">Frequently asked questions, read our [FAQ](active-directory-reporting-faq.md)</span></span> 

- <span data-ttu-id="e6bab-112">Kontrollera utfärdar [filen ett supportärende](active-directory-troubleshooting-support-howto.md)</span><span class="sxs-lookup"><span data-stu-id="e6bab-112">Issues, please [file a support ticket](active-directory-troubleshooting-support-howto.md)</span></span> 


## <a name="who-can-access-hello-data"></a><span data-ttu-id="e6bab-113">Vem som kan komma åt hello data?</span><span class="sxs-lookup"><span data-stu-id="e6bab-113">Who can access hello data?</span></span>
* <span data-ttu-id="e6bab-114">Användare med hello säkerhet Admin eller säkerhet Reader rollen</span><span class="sxs-lookup"><span data-stu-id="e6bab-114">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="e6bab-115">Globala administratörer</span><span class="sxs-lookup"><span data-stu-id="e6bab-115">Global Admins</span></span>
* <span data-ttu-id="e6bab-116">Alla appar som har tillståndet tooaccess hello API (auktorisering i apptjänst kan vara installationen endast baserat på Global administratör behörighet)</span><span class="sxs-lookup"><span data-stu-id="e6bab-116">Any app that has authorization tooaccess hello API (app authorization can be setup only based on Global Admin’s permission)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6bab-117">Krav</span><span class="sxs-lookup"><span data-stu-id="e6bab-117">Prerequisites</span></span>
<span data-ttu-id="e6bab-118">Hej Reporting API i ordning tooaccess detta rapporterar via, måste du ha:</span><span class="sxs-lookup"><span data-stu-id="e6bab-118">In order tooaccess this report through hello Reporting API, you must have:</span></span>

* <span data-ttu-id="e6bab-119">En [Azure Active Directory ledigt eller bättre edition](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="e6bab-119">An [Azure Active Directory Free or better edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="e6bab-120">Slutförda hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="e6bab-120">Completed hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-hello-api"></a><span data-ttu-id="e6bab-121">Åtkomst till hello-API</span><span class="sxs-lookup"><span data-stu-id="e6bab-121">Accessing hello API</span></span>
<span data-ttu-id="e6bab-122">Du kan antingen använda detta API via hello [diagram Explorer](https://graphexplorer2.cloudapp.net) eller programmässigt med, till exempel PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6bab-122">You can either access this API through hello [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="e6bab-123">I ordning för PowerShell toocorrectly tolka hello OData filter syntax används i AAD diagram REST-anrop, måste du använda hello backtick (även kallat: allvarligt accent) tecken för ”undantagstecken” hello $.</span><span class="sxs-lookup"><span data-stu-id="e6bab-123">In order for PowerShell toocorrectly interpret hello OData filter syntax used in AAD Graph REST calls, you must use hello backtick (aka: grave accent) character too“escape” hello $ character.</span></span> <span data-ttu-id="e6bab-124">Hej backtick tecknet som fungerar som [PowerShells escape-tecknet](https://technet.microsoft.com/library/hh847755.aspx), vilket gör att PowerShell toodo en literal tolkning av hello $-tecken och undvika förvirrande som ett PowerShell-variabelnamn (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="e6bab-124">hello backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell toodo a literal interpretation of hello $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="e6bab-125">hello fokuserar i det här avsnittet på hello diagrammet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e6bab-125">hello focus of this topic is on hello Graph Explorer.</span></span> <span data-ttu-id="e6bab-126">Ett PowerShell-exempel finns [PowerShell-skriptet](active-directory-reporting-api-audit-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="e6bab-126">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-audit-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="e6bab-127">API-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e6bab-127">API Endpoint</span></span>
<span data-ttu-id="e6bab-128">Du kan komma åt den här API: et med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="e6bab-128">You can access this API using hello following URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

<span data-ttu-id="e6bab-129">Det finns ingen gräns för hello antalet poster som returneras av hello Azure AD granska API (med OData sidbrytning).</span><span class="sxs-lookup"><span data-stu-id="e6bab-129">There is no limit on hello number of records returned by hello Azure AD audit API (using OData pagination).</span></span>
<span data-ttu-id="e6bab-130">Kvarhållning begränsningar rapportdata, kolla [Reporting bevarandeprinciper](active-directory-reporting-retention.md).</span><span class="sxs-lookup"><span data-stu-id="e6bab-130">For retention limits on reporting data, check out [Reporting Retention Policies](active-directory-reporting-retention.md).</span></span>

<span data-ttu-id="e6bab-131">Det här anropet returnerar hello data i batchar.</span><span class="sxs-lookup"><span data-stu-id="e6bab-131">This call returns hello data in batches.</span></span> <span data-ttu-id="e6bab-132">Varje grupp som har högst 1000 poster.</span><span class="sxs-lookup"><span data-stu-id="e6bab-132">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="e6bab-133">tooget hello nästa batch poster, Använd hello nästa länk.</span><span class="sxs-lookup"><span data-stu-id="e6bab-133">tooget hello next batch of records, use hello Next link.</span></span> <span data-ttu-id="e6bab-134">Hämta hello skiptoken information från hello första uppsättning returnerade poster.</span><span class="sxs-lookup"><span data-stu-id="e6bab-134">Get hello skiptoken information from hello first set of returned records.</span></span> <span data-ttu-id="e6bab-135">hello skip-token kommer att hello slutet av hello resultatmängden.</span><span class="sxs-lookup"><span data-stu-id="e6bab-135">hello skip token will be at hello end of hello result set.</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a><span data-ttu-id="e6bab-136">Filter som stöds</span><span class="sxs-lookup"><span data-stu-id="e6bab-136">Supported filters</span></span>
<span data-ttu-id="e6bab-137">Du kan begränsa hello antalet poster som returneras av en API-anrop i form av ett filter.</span><span class="sxs-lookup"><span data-stu-id="e6bab-137">You can narrow down hello number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="e6bab-138">Relaterade data, hello följande filter stöds för inloggning API:</span><span class="sxs-lookup"><span data-stu-id="e6bab-138">For sign-in API related data, hello following filters are supported:</span></span>

* <span data-ttu-id="e6bab-139">**$top =\<antal poster toobe returneras\>**  -toolimit hello antalet returnerade poster.</span><span class="sxs-lookup"><span data-stu-id="e6bab-139">**$top=\<number of records toobe returned\>** - toolimit hello number of returned records.</span></span> <span data-ttu-id="e6bab-140">Det här är en kostsam åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e6bab-140">This is an expensive operation.</span></span> <span data-ttu-id="e6bab-141">Du bör inte använda det här filtret om du vill tooreturn tusentals objekt.</span><span class="sxs-lookup"><span data-stu-id="e6bab-141">You should not use this filter if you want tooreturn thousands of objects.</span></span>     
* <span data-ttu-id="e6bab-142">**$filter =\<filter-instruktionen\>**  -toospecify hello baserat på filter som stöds fält, hello typ av poster som intresserar dig</span><span class="sxs-lookup"><span data-stu-id="e6bab-142">**$filter=\<your filter statement\>** - toospecify, on hello basis of supported filter fields, hello type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="e6bab-143">Filter som stöds fält och operatorer</span><span class="sxs-lookup"><span data-stu-id="e6bab-143">Supported filter fields and operators</span></span>
<span data-ttu-id="e6bab-144">toospecify hello typ av poster som intresserar dig, kan du skapa ett filter-uttryck som kan innehålla en eller en kombination av hello följande filterfält:</span><span class="sxs-lookup"><span data-stu-id="e6bab-144">toospecify hello type of records you care about, you can build a filter statement that can contain either one or a combination of hello following filter fields:</span></span>

* <span data-ttu-id="e6bab-145">[activityDate](#activitydate) -definierar ett datum eller datumintervall</span><span class="sxs-lookup"><span data-stu-id="e6bab-145">[activityDate](#activitydate)  - defines a date or date range</span></span>
* <span data-ttu-id="e6bab-146">[kategori](#category) -definierar hello kategori toofilter på.</span><span class="sxs-lookup"><span data-stu-id="e6bab-146">[category](#category) - defines hello category you want toofilter on.</span></span>
* <span data-ttu-id="e6bab-147">[activityStatus](#activitystatus) -definierar hello status för en aktivitet</span><span class="sxs-lookup"><span data-stu-id="e6bab-147">[activityStatus](#activitystatus) - defines hello status of an activity</span></span>
* <span data-ttu-id="e6bab-148">[activityType](#activitytype) -definierar hello-typ för en aktivitet</span><span class="sxs-lookup"><span data-stu-id="e6bab-148">[activityType](#activitytype)  - defines hello type of an activity</span></span>
* <span data-ttu-id="e6bab-149">[aktiviteten](#activity) -definierar hello aktivitet som sträng</span><span class="sxs-lookup"><span data-stu-id="e6bab-149">[activity](#activity) - defines hello activity as string</span></span>  
* <span data-ttu-id="e6bab-150">[namn påskådespelare/](#actorname) -definierar hello aktören i form av hello aktören namn</span><span class="sxs-lookup"><span data-stu-id="e6bab-150">[actor/name](#actorname) -   defines hello actor in form of hello actor's name</span></span>
* <span data-ttu-id="e6bab-151">[aktören/objectid](#actorobjectid) -definierar hello aktören i form av hello aktörs-ID</span><span class="sxs-lookup"><span data-stu-id="e6bab-151">[actor/objectid](#actorobjectid) - defines hello actor in form of hello actor's ID</span></span>   
* <span data-ttu-id="e6bab-152">[aktören/upn](#actorupn) -definierar hello aktören i form av hello aktören principen (user Principal name)</span><span class="sxs-lookup"><span data-stu-id="e6bab-152">[actor/upn](#actorupn)  - defines hello actor in form of hello actor's user principle name (UPN)</span></span> 
* <span data-ttu-id="e6bab-153">[målnamn/](#targetname) -definierar hello mål i form av hello aktören namn</span><span class="sxs-lookup"><span data-stu-id="e6bab-153">[target/name](#targetname)  - defines hello target in form of hello actor's name</span></span>
* <span data-ttu-id="e6bab-154">[mål/objectid](#targetobjectid) -definierar hello mål i form av hello mål-ID</span><span class="sxs-lookup"><span data-stu-id="e6bab-154">[target/objectid](#targetobjectid) - defines hello target in form of hello target's ID</span></span>  
* <span data-ttu-id="e6bab-155">[mål/upn](#targetupn) -definierar hello aktören i form av hello aktören principen (user Principal name)</span><span class="sxs-lookup"><span data-stu-id="e6bab-155">[target/upn](#targetupn) - defines hello actor in form of hello actor's user principle name (UPN)</span></span>   

- - -
### <a name="activitydate"></a><span data-ttu-id="e6bab-156">activityDate</span><span class="sxs-lookup"><span data-stu-id="e6bab-156">activityDate</span></span>
<span data-ttu-id="e6bab-157">**Stöd för operatörer**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="e6bab-157">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="e6bab-158">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-158">**Example**:</span></span>

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

<span data-ttu-id="e6bab-159">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-159">**Notes**:</span></span>

<span data-ttu-id="e6bab-160">datetime ska vara i UTC-format</span><span class="sxs-lookup"><span data-stu-id="e6bab-160">datetime should be in UTC format</span></span>

- - -
### <a name="category"></a><span data-ttu-id="e6bab-161">category</span><span class="sxs-lookup"><span data-stu-id="e6bab-161">category</span></span>

<span data-ttu-id="e6bab-162">**Värden som stöds**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-162">**Supported values**:</span></span>

| <span data-ttu-id="e6bab-163">Kategori</span><span class="sxs-lookup"><span data-stu-id="e6bab-163">Category</span></span>                         | <span data-ttu-id="e6bab-164">Värde</span><span class="sxs-lookup"><span data-stu-id="e6bab-164">Value</span></span>     |
| :--                              | ---       |
| <span data-ttu-id="e6bab-165">Kärnkatalog</span><span class="sxs-lookup"><span data-stu-id="e6bab-165">Core Directory</span></span>                   | <span data-ttu-id="e6bab-166">Katalog</span><span class="sxs-lookup"><span data-stu-id="e6bab-166">Directory</span></span> |
| <span data-ttu-id="e6bab-167">Hantering av lösenord för självbetjäning</span><span class="sxs-lookup"><span data-stu-id="e6bab-167">Self-service Password Management</span></span> | <span data-ttu-id="e6bab-168">SSPR</span><span class="sxs-lookup"><span data-stu-id="e6bab-168">SSPR</span></span>      |
| <span data-ttu-id="e6bab-169">Självbetjäning, grupphantering</span><span class="sxs-lookup"><span data-stu-id="e6bab-169">Self-service Group Management</span></span>    | <span data-ttu-id="e6bab-170">SSGM</span><span class="sxs-lookup"><span data-stu-id="e6bab-170">SSGM</span></span>      |
| <span data-ttu-id="e6bab-171">Kontoetablering</span><span class="sxs-lookup"><span data-stu-id="e6bab-171">Account Provisioning</span></span>             | <span data-ttu-id="e6bab-172">Sync</span><span class="sxs-lookup"><span data-stu-id="e6bab-172">Sync</span></span>      |
| <span data-ttu-id="e6bab-173">Automatiserad förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="e6bab-173">Automated Password Rollover</span></span>      | <span data-ttu-id="e6bab-174">Automatiserad förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="e6bab-174">Automated Password Rollover</span></span> |
| <span data-ttu-id="e6bab-175">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="e6bab-175">Identity Protection</span></span>              | <span data-ttu-id="e6bab-176">IdentityProtection</span><span class="sxs-lookup"><span data-stu-id="e6bab-176">IdentityProtection</span></span> |
| <span data-ttu-id="e6bab-177">Inbjudna användare</span><span class="sxs-lookup"><span data-stu-id="e6bab-177">Invited Users</span></span>                    | <span data-ttu-id="e6bab-178">Inbjudna användare</span><span class="sxs-lookup"><span data-stu-id="e6bab-178">Invited Users</span></span> |
| <span data-ttu-id="e6bab-179">MIM-tjänst</span><span class="sxs-lookup"><span data-stu-id="e6bab-179">MIM Service</span></span>                      | <span data-ttu-id="e6bab-180">MIM-tjänst</span><span class="sxs-lookup"><span data-stu-id="e6bab-180">MIM Service</span></span> |



<span data-ttu-id="e6bab-181">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e6bab-181">**Supported operators**: eq</span></span>

<span data-ttu-id="e6bab-182">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-182">**Example**:</span></span>

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a><span data-ttu-id="e6bab-183">ActivityStatus</span><span class="sxs-lookup"><span data-stu-id="e6bab-183">activityStatus</span></span>

<span data-ttu-id="e6bab-184">**Värden som stöds**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-184">**Supported values**:</span></span>

| <span data-ttu-id="e6bab-185">Aktivitetsstatus</span><span class="sxs-lookup"><span data-stu-id="e6bab-185">Activity Status</span></span> | <span data-ttu-id="e6bab-186">Värde</span><span class="sxs-lookup"><span data-stu-id="e6bab-186">Value</span></span> |
| :--             | ---   |
| <span data-ttu-id="e6bab-187">Lyckades</span><span class="sxs-lookup"><span data-stu-id="e6bab-187">Success</span></span>         | <span data-ttu-id="e6bab-188">0</span><span class="sxs-lookup"><span data-stu-id="e6bab-188">0</span></span>     |
| <span data-ttu-id="e6bab-189">Fel</span><span class="sxs-lookup"><span data-stu-id="e6bab-189">Failure</span></span>         | <span data-ttu-id="e6bab-190">- 1</span><span class="sxs-lookup"><span data-stu-id="e6bab-190">- 1</span></span>   |

<span data-ttu-id="e6bab-191">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e6bab-191">**Supported operators**: eq</span></span>

<span data-ttu-id="e6bab-192">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-192">**Example**:</span></span>

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a><span data-ttu-id="e6bab-193">activityType</span><span class="sxs-lookup"><span data-stu-id="e6bab-193">activityType</span></span>
<span data-ttu-id="e6bab-194">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e6bab-194">**Supported operators**: eq</span></span>

<span data-ttu-id="e6bab-195">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-195">**Example**:</span></span>

    $filter=activityType eq 'User'    

<span data-ttu-id="e6bab-196">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-196">**Notes**:</span></span>

<span data-ttu-id="e6bab-197">skiftlägeskänsligt</span><span class="sxs-lookup"><span data-stu-id="e6bab-197">case-sensitive</span></span>

- - -
### <a name="activity"></a><span data-ttu-id="e6bab-198">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="e6bab-198">activity</span></span>
<span data-ttu-id="e6bab-199">**Stöd för operatörer**: eq, innehåller startsWith</span><span class="sxs-lookup"><span data-stu-id="e6bab-199">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="e6bab-200">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-200">**Example**:</span></span>

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

<span data-ttu-id="e6bab-201">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-201">**Notes**:</span></span>

<span data-ttu-id="e6bab-202">skiftlägeskänsligt</span><span class="sxs-lookup"><span data-stu-id="e6bab-202">case-sensitive</span></span>

- - -
### <a name="actorname"></a><span data-ttu-id="e6bab-203">aktören/namn</span><span class="sxs-lookup"><span data-stu-id="e6bab-203">actor/name</span></span>
<span data-ttu-id="e6bab-204">**Stöd för operatörer**: eq, innehåller startsWith</span><span class="sxs-lookup"><span data-stu-id="e6bab-204">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="e6bab-205">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-205">**Example**:</span></span>

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

<span data-ttu-id="e6bab-206">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-206">**Notes**:</span></span>

<span data-ttu-id="e6bab-207">Icke skiftlägeskänslig</span><span class="sxs-lookup"><span data-stu-id="e6bab-207">case-insensitive</span></span>

- - -
### <a name="actorobjectid"></a><span data-ttu-id="e6bab-208">aktören/objectId</span><span class="sxs-lookup"><span data-stu-id="e6bab-208">actor/objectId</span></span>
<span data-ttu-id="e6bab-209">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e6bab-209">**Supported operators**: eq</span></span>

<span data-ttu-id="e6bab-210">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-210">**Example**:</span></span>

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a><span data-ttu-id="e6bab-211">målnamn /</span><span class="sxs-lookup"><span data-stu-id="e6bab-211">target/name</span></span>
<span data-ttu-id="e6bab-212">**Stöd för operatörer**: eq, innehåller startsWith</span><span class="sxs-lookup"><span data-stu-id="e6bab-212">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="e6bab-213">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-213">**Example**:</span></span>

    $filter=targets/any(t: t/name eq 'some name')    

<span data-ttu-id="e6bab-214">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-214">**Notes**:</span></span>

<span data-ttu-id="e6bab-215">Icke skiftlägeskänslig</span><span class="sxs-lookup"><span data-stu-id="e6bab-215">Case-insensitive</span></span>

- - -
### <a name="targetupn"></a><span data-ttu-id="e6bab-216">mål/upn</span><span class="sxs-lookup"><span data-stu-id="e6bab-216">target/upn</span></span>
<span data-ttu-id="e6bab-217">**Stöd för operatörer**: eq, startsWith</span><span class="sxs-lookup"><span data-stu-id="e6bab-217">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="e6bab-218">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-218">**Example**:</span></span>

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

<span data-ttu-id="e6bab-219">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-219">**Notes**:</span></span>

* <span data-ttu-id="e6bab-220">Icke skiftlägeskänslig</span><span class="sxs-lookup"><span data-stu-id="e6bab-220">Case-insensitive</span></span>
* <span data-ttu-id="e6bab-221">Du behöver tooadd hello fullständiga namnområde när du frågar Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity</span><span class="sxs-lookup"><span data-stu-id="e6bab-221">You need tooadd hello full namespace when querying  Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity</span></span>

- - -
### <a name="targetobjectid"></a><span data-ttu-id="e6bab-222">mål/objectId</span><span class="sxs-lookup"><span data-stu-id="e6bab-222">target/objectId</span></span>
<span data-ttu-id="e6bab-223">**Stöd för operatörer**: eq</span><span class="sxs-lookup"><span data-stu-id="e6bab-223">**Supported operators**: eq</span></span>

<span data-ttu-id="e6bab-224">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-224">**Example**:</span></span>

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a><span data-ttu-id="e6bab-225">aktören/upn</span><span class="sxs-lookup"><span data-stu-id="e6bab-225">actor/upn</span></span>
<span data-ttu-id="e6bab-226">**Stöd för operatörer**: eq, startsWith</span><span class="sxs-lookup"><span data-stu-id="e6bab-226">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="e6bab-227">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-227">**Example**:</span></span>

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

<span data-ttu-id="e6bab-228">**Anteckningar**:</span><span class="sxs-lookup"><span data-stu-id="e6bab-228">**Notes**:</span></span>

* <span data-ttu-id="e6bab-229">Icke skiftlägeskänslig</span><span class="sxs-lookup"><span data-stu-id="e6bab-229">Case-insensitive</span></span> 
* <span data-ttu-id="e6bab-230">Du behöver tooadd hello fullständiga namnområde när du frågar Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity</span><span class="sxs-lookup"><span data-stu-id="e6bab-230">You need tooadd hello full namespace when querying Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="e6bab-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6bab-231">Next Steps</span></span>
* <span data-ttu-id="e6bab-232">Vill du toosee exempel för filtrerade systemaktiviteter?</span><span class="sxs-lookup"><span data-stu-id="e6bab-232">Do you want toosee examples for filtered system activities?</span></span> <span data-ttu-id="e6bab-233">Kolla in hello [Azure Active Directory audit API-exempel](active-directory-reporting-api-audit-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e6bab-233">Check out hello [Azure Active Directory audit API samples](active-directory-reporting-api-audit-samples.md).</span></span>
* <span data-ttu-id="e6bab-234">Vill du tooknow mer om hello Azure AD reporting API?</span><span class="sxs-lookup"><span data-stu-id="e6bab-234">Do you want tooknow more about hello Azure AD reporting API?</span></span> <span data-ttu-id="e6bab-235">Se [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="e6bab-235">See [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

