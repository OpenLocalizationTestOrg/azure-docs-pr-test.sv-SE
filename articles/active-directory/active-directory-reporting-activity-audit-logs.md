---
title: Granska aktivitetsrapporter i Azure Active Directory-portalen | Microsoft Docs
description: Introduktion till granskning av aktivitetsrapporter i Azure Active Directory-portalen
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f2d0332d815c82d7d47625e020de2e9c5099deeb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a><span data-ttu-id="c78b7-103">Granska aktivitetsrapporter i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="c78b7-103">Audit activity reports in the Azure Active Directory portal</span></span> 

<span data-ttu-id="c78b7-104">Med rapportering i Azure Active Directory (Azure AD) får du all information du behöver för att ta reda på hur din miljö klarar sig.</span><span class="sxs-lookup"><span data-stu-id="c78b7-104">With reporting in Azure Active Directory (Azure AD), you can get the information you need to determine how your environment is doing.</span></span>

<span data-ttu-id="c78b7-105">Rapporteringsarkitekturen i Azure AD består av följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="c78b7-105">The reporting architecture in Azure AD consists of the following components:</span></span>

- <span data-ttu-id="c78b7-106">**Aktivitet**</span><span class="sxs-lookup"><span data-stu-id="c78b7-106">**Activity**</span></span> 
    - <span data-ttu-id="c78b7-107">**Inloggningsaktiviteter** – Information om användningen av hanterade program och användares inloggningsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="c78b7-107">**Sign-in activities** – Information about the usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="c78b7-108">**Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c78b7-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="c78b7-109">**Säkerhet**</span><span class="sxs-lookup"><span data-stu-id="c78b7-109">**Security**</span></span> 
    - <span data-ttu-id="c78b7-110">**Riskfyllda inloggningar** – En riskfylld inloggning indikerar ett potentiellt inloggningsförsök av någon annan än användarkontots ägare.</span><span class="sxs-lookup"><span data-stu-id="c78b7-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> <span data-ttu-id="c78b7-111">Mer information finns i avsnittet om riskfyllda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="c78b7-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="c78b7-112">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="c78b7-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="c78b7-113">Mer information finns i avsnittet om användare som har flaggats för risk.</span><span class="sxs-lookup"><span data-stu-id="c78b7-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="c78b7-114">I det här ämnet får du en översikt över granskningsaktiviteterna.</span><span class="sxs-lookup"><span data-stu-id="c78b7-114">This topic gives you an overview of the audit activities.</span></span>
 
## <a name="who-can-access-the-data"></a><span data-ttu-id="c78b7-115">Vem kan komma åt dessa data?</span><span class="sxs-lookup"><span data-stu-id="c78b7-115">Who can access the data?</span></span>
* <span data-ttu-id="c78b7-116">Användare i rollen säkerhetsadministratör eller säkerhetsläsare</span><span class="sxs-lookup"><span data-stu-id="c78b7-116">Users in the Security Admin or Security Reader role</span></span>
* <span data-ttu-id="c78b7-117">Globala administratörer</span><span class="sxs-lookup"><span data-stu-id="c78b7-117">Global Admins</span></span>
* <span data-ttu-id="c78b7-118">Enskilda användare (icke-administratörer) kan se sina egna aktiviteter</span><span class="sxs-lookup"><span data-stu-id="c78b7-118">Individual users (non-admins) can see their own activities</span></span>


## <a name="audit-logs"></a><span data-ttu-id="c78b7-119">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="c78b7-119">Audit logs</span></span>

<span data-ttu-id="c78b7-120">Granskningsloggarna i Azure Active Directory ger dokumentation över systemaktiviteter för kontroll av överensstämmelse.</span><span class="sxs-lookup"><span data-stu-id="c78b7-120">The audit logs in Azure Active Directory provide records of system activities for compliance.</span></span>  
<span data-ttu-id="c78b7-121">Din startpunkt för alla granskningsdata är **Granskningsloggar** i avsnittet **Aktivitet** i **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c78b7-121">Your first entry point to all auditing data is **Audit logs** in the **Activity** section of **Azure Active Directory**.</span></span>

<span data-ttu-id="c78b7-122">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/61.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-122">![Audit logs](./media/active-directory-reporting-activity-audit-logs/61.png "Audit logs")</span></span>

<span data-ttu-id="c78b7-123">En granskningslogg har en standardlistvy som visar:</span><span class="sxs-lookup"><span data-stu-id="c78b7-123">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="c78b7-124">datum och tid för förekomsten</span><span class="sxs-lookup"><span data-stu-id="c78b7-124">the date and time of the occurrence</span></span>
- <span data-ttu-id="c78b7-125">initieraren/aktören (*vem*) för en aktivitet</span><span class="sxs-lookup"><span data-stu-id="c78b7-125">the initiator / actor (*who*) of an activity</span></span> 
- <span data-ttu-id="c78b7-126">aktiviteten (*vad*)</span><span class="sxs-lookup"><span data-stu-id="c78b7-126">the activity (*what*)</span></span> 
- <span data-ttu-id="c78b7-127">målet</span><span class="sxs-lookup"><span data-stu-id="c78b7-127">the target</span></span>

<span data-ttu-id="c78b7-128">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/18.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-128">![Audit logs](./media/active-directory-reporting-activity-audit-logs/18.png "Audit logs")</span></span>

<span data-ttu-id="c78b7-129">Du kan anpassa listvyn genom att klicka på **Kolumner** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="c78b7-129">You can customize the list view by clicking **Columns** in the toolbar.</span></span>

<span data-ttu-id="c78b7-130">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/19.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-130">![Audit logs](./media/active-directory-reporting-activity-audit-logs/19.png "Audit logs")</span></span>

<span data-ttu-id="c78b7-131">På så sätt kan du visa ytterligare fält eller ta bort fält som redan visas.</span><span class="sxs-lookup"><span data-stu-id="c78b7-131">This enables you to display additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="c78b7-132">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/21.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-132">![Audit logs](./media/active-directory-reporting-activity-audit-logs/21.png "Audit logs")</span></span>


<span data-ttu-id="c78b7-133">När du klickar på ett objekt i listvyn visas all tillgänglig information om det.</span><span class="sxs-lookup"><span data-stu-id="c78b7-133">By clicking an item in the list view, you get all available details about it.</span></span>

<span data-ttu-id="c78b7-134">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/22.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-134">![Audit logs](./media/active-directory-reporting-activity-audit-logs/22.png "Audit logs")</span></span>


## <a name="filtering-audit-logs"></a><span data-ttu-id="c78b7-135">Filtrera granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="c78b7-135">Filtering audit logs</span></span>

<span data-ttu-id="c78b7-136">Om du vill begränsa de data som rapporteras till en nivå som passar dig kan du filtrera granskningsdata med hjälp av följande fält:</span><span class="sxs-lookup"><span data-stu-id="c78b7-136">To narrow down the reported data to a level that works for you, you can filter the audit data using the following fields:</span></span>

- <span data-ttu-id="c78b7-137">Datumintervall</span><span class="sxs-lookup"><span data-stu-id="c78b7-137">Date range</span></span>
- <span data-ttu-id="c78b7-138">Initierad av (aktör)</span><span class="sxs-lookup"><span data-stu-id="c78b7-138">Initiated by (Actor)</span></span>
- <span data-ttu-id="c78b7-139">Kategori</span><span class="sxs-lookup"><span data-stu-id="c78b7-139">Category</span></span>
- <span data-ttu-id="c78b7-140">Resurstyp för aktivitet</span><span class="sxs-lookup"><span data-stu-id="c78b7-140">Activity resource type</span></span>
- <span data-ttu-id="c78b7-141">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="c78b7-141">Activity</span></span>

<span data-ttu-id="c78b7-142">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/23.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-142">![Audit logs](./media/active-directory-reporting-activity-audit-logs/23.png "Audit logs")</span></span>


<span data-ttu-id="c78b7-143">Med filtret för **datumintervall** kan du definiera en tidsram för de data som returneras.</span><span class="sxs-lookup"><span data-stu-id="c78b7-143">The **date range** filter enables to you to define a timeframe for the returned data.</span></span>  
<span data-ttu-id="c78b7-144">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="c78b7-144">Possible values are:</span></span>

- <span data-ttu-id="c78b7-145">1 månad</span><span class="sxs-lookup"><span data-stu-id="c78b7-145">1 month</span></span>
- <span data-ttu-id="c78b7-146">7 dagar</span><span class="sxs-lookup"><span data-stu-id="c78b7-146">7 days</span></span>
- <span data-ttu-id="c78b7-147">24 timmar</span><span class="sxs-lookup"><span data-stu-id="c78b7-147">24 hours</span></span>
- <span data-ttu-id="c78b7-148">Anpassat</span><span class="sxs-lookup"><span data-stu-id="c78b7-148">Custom</span></span>

<span data-ttu-id="c78b7-149">När du väljer en anpassad tidsram kan du konfigurera en starttid och en sluttid.</span><span class="sxs-lookup"><span data-stu-id="c78b7-149">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="c78b7-150">Med filtret **initierad av** kan du definiera en aktörs namn eller dess UPN (Universal Principal Name).</span><span class="sxs-lookup"><span data-stu-id="c78b7-150">The **initiated by** filter enables you to define an actor's name or its universal principal name (UPN).</span></span>

<span data-ttu-id="c78b7-151">Med filtret **kategori** kan du välja något av följande filter:</span><span class="sxs-lookup"><span data-stu-id="c78b7-151">The **category** filter enables you to select one of the following filter:</span></span>

- <span data-ttu-id="c78b7-152">Alla</span><span class="sxs-lookup"><span data-stu-id="c78b7-152">All</span></span>
- <span data-ttu-id="c78b7-153">Grundläggande kategori</span><span class="sxs-lookup"><span data-stu-id="c78b7-153">Core category</span></span>
- <span data-ttu-id="c78b7-154">Grundläggande katalog</span><span class="sxs-lookup"><span data-stu-id="c78b7-154">Core directory</span></span>
- <span data-ttu-id="c78b7-155">Lösenordshantering via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="c78b7-155">Self-service password management</span></span>
- <span data-ttu-id="c78b7-156">Självbetjäning, grupphantering</span><span class="sxs-lookup"><span data-stu-id="c78b7-156">Self-service group management</span></span>
- <span data-ttu-id="c78b7-157">Kontoetablering – Automatiserad förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="c78b7-157">Account provisioning- Automated password rollover</span></span>
- <span data-ttu-id="c78b7-158">Inbjudna användare</span><span class="sxs-lookup"><span data-stu-id="c78b7-158">Invited users</span></span>
- <span data-ttu-id="c78b7-159">MIM-tjänst</span><span class="sxs-lookup"><span data-stu-id="c78b7-159">MIM service</span></span>
- <span data-ttu-id="c78b7-160">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="c78b7-160">Identity Protection</span></span>
- <span data-ttu-id="c78b7-161">B2C</span><span class="sxs-lookup"><span data-stu-id="c78b7-161">B2C</span></span>

<span data-ttu-id="c78b7-162">Med filtret för **aktivitsresurstyp** kan du välja något av följande filter:</span><span class="sxs-lookup"><span data-stu-id="c78b7-162">The **activity resource type** filter enables you to select one of the following filters:</span></span>

- <span data-ttu-id="c78b7-163">Alla</span><span class="sxs-lookup"><span data-stu-id="c78b7-163">All</span></span> 
- <span data-ttu-id="c78b7-164">Grupp</span><span class="sxs-lookup"><span data-stu-id="c78b7-164">Group</span></span>
- <span data-ttu-id="c78b7-165">Katalog</span><span class="sxs-lookup"><span data-stu-id="c78b7-165">Directory</span></span>
- <span data-ttu-id="c78b7-166">Användare</span><span class="sxs-lookup"><span data-stu-id="c78b7-166">User</span></span>
- <span data-ttu-id="c78b7-167">Program</span><span class="sxs-lookup"><span data-stu-id="c78b7-167">Application</span></span>
- <span data-ttu-id="c78b7-168">Princip</span><span class="sxs-lookup"><span data-stu-id="c78b7-168">Policy</span></span>
- <span data-ttu-id="c78b7-169">Enhet</span><span class="sxs-lookup"><span data-stu-id="c78b7-169">Device</span></span>
- <span data-ttu-id="c78b7-170">Annat</span><span class="sxs-lookup"><span data-stu-id="c78b7-170">Other</span></span>

<span data-ttu-id="c78b7-171">När du väljer **Grupp** som **aktivitetsresurstyp** får du tillgång till ytterligare en filterkategori som du kan använda för att även ange en **källa**:</span><span class="sxs-lookup"><span data-stu-id="c78b7-171">When you select **Group** as **activity resource type**, you get an additional filter category that enables you to also provide a **Source**:</span></span>

- <span data-ttu-id="c78b7-172">Azure AD</span><span class="sxs-lookup"><span data-stu-id="c78b7-172">Azure AD</span></span>
- <span data-ttu-id="c78b7-173">O365</span><span class="sxs-lookup"><span data-stu-id="c78b7-173">O365</span></span>


<span data-ttu-id="c78b7-174">Filtret **aktivitet** baseras på kategorin och den aktivitetsresurstyp som du väljer.</span><span class="sxs-lookup"><span data-stu-id="c78b7-174">The **activity** filter is based on the category and Activity resource type selection you make.</span></span> <span data-ttu-id="c78b7-175">Du kan välja en specifik aktivitet som du vill visa eller välja alla.</span><span class="sxs-lookup"><span data-stu-id="c78b7-175">You can select a specific activity you want to see or choose all.</span></span> 

<span data-ttu-id="c78b7-176">Du kan hämta listan över alla granskningsaktiviteter med Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, där $tenantdomain = ditt domännamn eller refererar till artikeln [Granska rapporthändelser](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="c78b7-176">You can get the list of all Audit Activities using the Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, where $tenantdomain = your domain name or refer to the article [audit report events](active-directory-reporting-audit-events.md).</span></span>


## <a name="audit-logs-shortcuts"></a><span data-ttu-id="c78b7-177">Genvägar till granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="c78b7-177">Audit logs shortcuts</span></span>

<span data-ttu-id="c78b7-178">Förutom **Azure Active Directory** finns det ytterligare två ställen på Azure Portal där du kan granska data:</span><span class="sxs-lookup"><span data-stu-id="c78b7-178">In addition to **Azure Active Directory**, the Azure portal provides you with two additional entry points to audit data:</span></span>

- <span data-ttu-id="c78b7-179">Användare och grupper</span><span class="sxs-lookup"><span data-stu-id="c78b7-179">Users and groups</span></span>
- <span data-ttu-id="c78b7-180">Företagsprogram</span><span class="sxs-lookup"><span data-stu-id="c78b7-180">Enterprise applications</span></span>

### <a name="users-and-groups-audit-logs"></a><span data-ttu-id="c78b7-181">Granskningsloggar för användare och grupper</span><span class="sxs-lookup"><span data-stu-id="c78b7-181">Users and groups audit logs</span></span>

<span data-ttu-id="c78b7-182">Med användar- och gruppbaserade granskningsrapporter kan du få svar på frågor som:</span><span class="sxs-lookup"><span data-stu-id="c78b7-182">With user and group-based audit reports, you can get answers to questions such as:</span></span>

- <span data-ttu-id="c78b7-183">Vilka typer av uppdateringar har getts användare?</span><span class="sxs-lookup"><span data-stu-id="c78b7-183">What types of updates have been applied the users?</span></span>

- <span data-ttu-id="c78b7-184">Hur många användare ändrades?</span><span class="sxs-lookup"><span data-stu-id="c78b7-184">How many users were changed?</span></span>

- <span data-ttu-id="c78b7-185">Hur många lösenord ändrades?</span><span class="sxs-lookup"><span data-stu-id="c78b7-185">How many passwords were changed?</span></span>

- <span data-ttu-id="c78b7-186">Vad har en administratör gjort i en katalog?</span><span class="sxs-lookup"><span data-stu-id="c78b7-186">What has an administrator done in a directory?</span></span>

- <span data-ttu-id="c78b7-187">Vilka grupper har lagts till?</span><span class="sxs-lookup"><span data-stu-id="c78b7-187">What are the groups that have been added?</span></span>

- <span data-ttu-id="c78b7-188">Finns det grupper med ändringar i medlemskap?</span><span class="sxs-lookup"><span data-stu-id="c78b7-188">Are there groups with membership changes?</span></span>

- <span data-ttu-id="c78b7-189">Har ägare av en grupp ändrats?</span><span class="sxs-lookup"><span data-stu-id="c78b7-189">Have the owners of group been changed?</span></span>

- <span data-ttu-id="c78b7-190">Vilka licenser har tilldelats en grupp eller en användare?</span><span class="sxs-lookup"><span data-stu-id="c78b7-190">What licenses have been assigned to a group or a user?</span></span>

<span data-ttu-id="c78b7-191">Om du bara vill kontrollera granskningsdata relaterade till användare och grupper finns det en filtrerad vy under **Granskningsloggar** i avsnittet **Aktivitet** i **Användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c78b7-191">If you just want to review auditing data that is related to users and groups, you can find a filtered view under **Audit logs** in the **Activity** section of the **Users and Groups**.</span></span> <span data-ttu-id="c78b7-192">I det här området är **Användare och grupper** den förvalda **aktivitetsresurstypen**.</span><span class="sxs-lookup"><span data-stu-id="c78b7-192">This entry point has **Users and groups** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="c78b7-193">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/93.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-193">![Audit logs](./media/active-directory-reporting-activity-audit-logs/93.png "Audit logs")</span></span>

### <a name="enterprise-applications-audit-logs"></a><span data-ttu-id="c78b7-194">Granskningsloggar för företagsprogram</span><span class="sxs-lookup"><span data-stu-id="c78b7-194">Enterprise applications audit logs</span></span>

<span data-ttu-id="c78b7-195">Med programbaserade granskningsrapporter kan du få svar på frågor som:</span><span class="sxs-lookup"><span data-stu-id="c78b7-195">With application-based audit reports, you can get answers to questions such as:</span></span>

* <span data-ttu-id="c78b7-196">Vilka program har lagts till eller uppdaterats?</span><span class="sxs-lookup"><span data-stu-id="c78b7-196">What are the applications that have been added or updated?</span></span>
* <span data-ttu-id="c78b7-197">Vilka program har tagits bort?</span><span class="sxs-lookup"><span data-stu-id="c78b7-197">What are the applications that have been removed?</span></span>
* <span data-ttu-id="c78b7-198">Har tjänstprincipen för ett program ändrats?</span><span class="sxs-lookup"><span data-stu-id="c78b7-198">Has a service principle for an application changed?</span></span>
* <span data-ttu-id="c78b7-199">Har namnen på program ändrats?</span><span class="sxs-lookup"><span data-stu-id="c78b7-199">Have the names of applications been changed?</span></span>
* <span data-ttu-id="c78b7-200">Vem gav tillstånd till ett program?</span><span class="sxs-lookup"><span data-stu-id="c78b7-200">Who gave consent to an application?</span></span>

<span data-ttu-id="c78b7-201">Om du bara vill kontrollera granskningsdata relaterade till dina program finns det en filtrerad vy under **Granskningsloggar** i avsnittet **Aktivitet** på bladet **Företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c78b7-201">If you just want to review auditing data that is related to your applications, you can find a filtered view under **Audit logs** in the **Activity** section of the **Enterprise applications** blade.</span></span> <span data-ttu-id="c78b7-202">I det här området är **Företagsprogram** den förvalda **aktivitetsresurstypen**.</span><span class="sxs-lookup"><span data-stu-id="c78b7-202">This entry point has **Enterprise applications** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="c78b7-203">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/134.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-203">![Audit logs](./media/active-directory-reporting-activity-audit-logs/134.png "Audit logs")</span></span>

<span data-ttu-id="c78b7-204">Du kan filtrera den här vyn ytterligare till bara **grupper** eller bara **användare**.</span><span class="sxs-lookup"><span data-stu-id="c78b7-204">You can filter this view further down to just **groups** or just **users**.</span></span>

<span data-ttu-id="c78b7-205">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/25.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="c78b7-205">![Audit logs](./media/active-directory-reporting-activity-audit-logs/25.png "Audit logs")</span></span>


## <a name="next-steps"></a><span data-ttu-id="c78b7-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c78b7-206">Next steps</span></span>

<span data-ttu-id="c78b7-207">En översikt över rapportering finns i [Azure Active Directory-rapportering](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c78b7-207">For an overview of reporting, see the [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>

