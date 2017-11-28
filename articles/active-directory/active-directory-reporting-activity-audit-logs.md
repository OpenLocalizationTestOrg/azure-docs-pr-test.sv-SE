---
title: aaaAudit aktivitetsrapporter hello Azure Active Directory-portalen | Microsoft Docs
description: Introduktion toohello granska aktivitetsrapporter hello Azure Active Directory-portalen
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
ms.openlocfilehash: 1567673f5030fc707b017c069f2ba7587962e5cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="audit-activity-reports-in-hello-azure-active-directory-portal"></a><span data-ttu-id="3eff5-103">Granska aktivitetsrapporter hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="3eff5-103">Audit activity reports in hello Azure Active Directory portal</span></span> 

<span data-ttu-id="3eff5-104">Du kan hämta hello information du behöver toodetermine hur din miljö fungerar med rapportering i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3eff5-104">With reporting in Azure Active Directory (Azure AD), you can get hello information you need toodetermine how your environment is doing.</span></span>

<span data-ttu-id="3eff5-105">Hej Rapporteringsarkitektur i Azure AD består av hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="3eff5-105">hello reporting architecture in Azure AD consists of hello following components:</span></span>

- <span data-ttu-id="3eff5-106">**Aktivitet**</span><span class="sxs-lookup"><span data-stu-id="3eff5-106">**Activity**</span></span> 
    - <span data-ttu-id="3eff5-107">**Logga in aktiviteter** – Information om hello användning av hanterade program och användaren loggar in aktiviteter</span><span class="sxs-lookup"><span data-stu-id="3eff5-107">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="3eff5-108">**Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3eff5-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="3eff5-109">**Säkerhet**</span><span class="sxs-lookup"><span data-stu-id="3eff5-109">**Security**</span></span> 
    - <span data-ttu-id="3eff5-110">**Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="3eff5-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="3eff5-111">Mer information finns i avsnittet om riskfyllda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="3eff5-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="3eff5-112">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="3eff5-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="3eff5-113">Mer information finns i avsnittet om användare som har flaggats för risk.</span><span class="sxs-lookup"><span data-stu-id="3eff5-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="3eff5-114">Det här avsnittet ger en översikt över hello audit aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3eff5-114">This topic gives you an overview of hello audit activities.</span></span>
 
## <a name="who-can-access-hello-data"></a><span data-ttu-id="3eff5-115">Vem som kan komma åt hello data?</span><span class="sxs-lookup"><span data-stu-id="3eff5-115">Who can access hello data?</span></span>
* <span data-ttu-id="3eff5-116">Användare med hello säkerhet Admin eller säkerhet Reader rollen</span><span class="sxs-lookup"><span data-stu-id="3eff5-116">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="3eff5-117">Globala administratörer</span><span class="sxs-lookup"><span data-stu-id="3eff5-117">Global Admins</span></span>
* <span data-ttu-id="3eff5-118">Enskilda användare (icke-administratörer) kan se sina egna aktiviteter</span><span class="sxs-lookup"><span data-stu-id="3eff5-118">Individual users (non-admins) can see their own activities</span></span>


## <a name="audit-logs"></a><span data-ttu-id="3eff5-119">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="3eff5-119">Audit logs</span></span>

<span data-ttu-id="3eff5-120">hello granskningsloggar i Azure Active Directory innehåller poster för systemaktiviteter för kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="3eff5-120">hello audit logs in Azure Active Directory provide records of system activities for compliance.</span></span>  
<span data-ttu-id="3eff5-121">Din första posten punkt tooall granska data är **granskningsloggar** i hello **aktiviteten** avsnitt i **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3eff5-121">Your first entry point tooall auditing data is **Audit logs** in hello **Activity** section of **Azure Active Directory**.</span></span>

<span data-ttu-id="3eff5-122">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/61.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-122">![Audit logs](./media/active-directory-reporting-activity-audit-logs/61.png "Audit logs")</span></span>

<span data-ttu-id="3eff5-123">En granskningslogg har en standardlistvy som visar:</span><span class="sxs-lookup"><span data-stu-id="3eff5-123">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="3eff5-124">hello datum och tid för hello förekomst</span><span class="sxs-lookup"><span data-stu-id="3eff5-124">hello date and time of hello occurrence</span></span>
- <span data-ttu-id="3eff5-125">Hej initieraren / aktören (*som*) för en aktivitet</span><span class="sxs-lookup"><span data-stu-id="3eff5-125">hello initiator / actor (*who*) of an activity</span></span> 
- <span data-ttu-id="3eff5-126">Hej aktivitet (*vad*)</span><span class="sxs-lookup"><span data-stu-id="3eff5-126">hello activity (*what*)</span></span> 
- <span data-ttu-id="3eff5-127">hello mål</span><span class="sxs-lookup"><span data-stu-id="3eff5-127">hello target</span></span>

<span data-ttu-id="3eff5-128">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/18.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-128">![Audit logs](./media/active-directory-reporting-activity-audit-logs/18.png "Audit logs")</span></span>

<span data-ttu-id="3eff5-129">Du kan anpassa hello listvyn genom att klicka på **kolumner** i hello-verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="3eff5-129">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>

<span data-ttu-id="3eff5-130">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/19.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-130">![Audit logs](./media/active-directory-reporting-activity-audit-logs/19.png "Audit logs")</span></span>

<span data-ttu-id="3eff5-131">Detta gör att du toodisplay ytterligare fält eller ta bort fält som redan visas.</span><span class="sxs-lookup"><span data-stu-id="3eff5-131">This enables you toodisplay additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="3eff5-132">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/21.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-132">![Audit logs](./media/active-directory-reporting-activity-audit-logs/21.png "Audit logs")</span></span>


<span data-ttu-id="3eff5-133">Genom att klicka på ett objekt i listvyn hello, hämta alla information om den.</span><span class="sxs-lookup"><span data-stu-id="3eff5-133">By clicking an item in hello list view, you get all available details about it.</span></span>

<span data-ttu-id="3eff5-134">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/22.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-134">![Audit logs](./media/active-directory-reporting-activity-audit-logs/22.png "Audit logs")</span></span>


## <a name="filtering-audit-logs"></a><span data-ttu-id="3eff5-135">Filtrera granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="3eff5-135">Filtering audit logs</span></span>

<span data-ttu-id="3eff5-136">toonarrow ned hello rapporterade data tooa nivå som fungerar för dig, kan du filtrera hello granskningsdata med hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="3eff5-136">toonarrow down hello reported data tooa level that works for you, you can filter hello audit data using hello following fields:</span></span>

- <span data-ttu-id="3eff5-137">Datumintervall</span><span class="sxs-lookup"><span data-stu-id="3eff5-137">Date range</span></span>
- <span data-ttu-id="3eff5-138">Initierad av (aktör)</span><span class="sxs-lookup"><span data-stu-id="3eff5-138">Initiated by (Actor)</span></span>
- <span data-ttu-id="3eff5-139">Kategori</span><span class="sxs-lookup"><span data-stu-id="3eff5-139">Category</span></span>
- <span data-ttu-id="3eff5-140">Resurstyp för aktivitet</span><span class="sxs-lookup"><span data-stu-id="3eff5-140">Activity resource type</span></span>
- <span data-ttu-id="3eff5-141">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3eff5-141">Activity</span></span>

<span data-ttu-id="3eff5-142">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/23.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-142">![Audit logs](./media/active-directory-reporting-activity-audit-logs/23.png "Audit logs")</span></span>


<span data-ttu-id="3eff5-143">Hej **intervallet** filter aktiverar tooyou toodefine en tidsram för hello returnerade data.</span><span class="sxs-lookup"><span data-stu-id="3eff5-143">hello **date range** filter enables tooyou toodefine a timeframe for hello returned data.</span></span>  
<span data-ttu-id="3eff5-144">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="3eff5-144">Possible values are:</span></span>

- <span data-ttu-id="3eff5-145">1 månad</span><span class="sxs-lookup"><span data-stu-id="3eff5-145">1 month</span></span>
- <span data-ttu-id="3eff5-146">7 dagar</span><span class="sxs-lookup"><span data-stu-id="3eff5-146">7 days</span></span>
- <span data-ttu-id="3eff5-147">24 timmar</span><span class="sxs-lookup"><span data-stu-id="3eff5-147">24 hours</span></span>
- <span data-ttu-id="3eff5-148">Anpassat</span><span class="sxs-lookup"><span data-stu-id="3eff5-148">Custom</span></span>

<span data-ttu-id="3eff5-149">När du väljer en anpassad tidsram kan du konfigurera en starttid och en sluttid.</span><span class="sxs-lookup"><span data-stu-id="3eff5-149">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="3eff5-150">Hej **initieras av** filter kan du toodefine skådespelare, namn eller dess universal huvudnamn (UPN).</span><span class="sxs-lookup"><span data-stu-id="3eff5-150">hello **initiated by** filter enables you toodefine an actor's name or its universal principal name (UPN).</span></span>

<span data-ttu-id="3eff5-151">Hej **kategori** filter kan du tooselect något av följande filter hello:</span><span class="sxs-lookup"><span data-stu-id="3eff5-151">hello **category** filter enables you tooselect one of hello following filter:</span></span>

- <span data-ttu-id="3eff5-152">Alla</span><span class="sxs-lookup"><span data-stu-id="3eff5-152">All</span></span>
- <span data-ttu-id="3eff5-153">Grundläggande kategori</span><span class="sxs-lookup"><span data-stu-id="3eff5-153">Core category</span></span>
- <span data-ttu-id="3eff5-154">Grundläggande katalog</span><span class="sxs-lookup"><span data-stu-id="3eff5-154">Core directory</span></span>
- <span data-ttu-id="3eff5-155">Lösenordshantering via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="3eff5-155">Self-service password management</span></span>
- <span data-ttu-id="3eff5-156">Självbetjäning, grupphantering</span><span class="sxs-lookup"><span data-stu-id="3eff5-156">Self-service group management</span></span>
- <span data-ttu-id="3eff5-157">Kontoetablering – Automatiserad förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="3eff5-157">Account provisioning- Automated password rollover</span></span>
- <span data-ttu-id="3eff5-158">Inbjudna användare</span><span class="sxs-lookup"><span data-stu-id="3eff5-158">Invited users</span></span>
- <span data-ttu-id="3eff5-159">MIM-tjänst</span><span class="sxs-lookup"><span data-stu-id="3eff5-159">MIM service</span></span>
- <span data-ttu-id="3eff5-160">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="3eff5-160">Identity Protection</span></span>
- <span data-ttu-id="3eff5-161">B2C</span><span class="sxs-lookup"><span data-stu-id="3eff5-161">B2C</span></span>

<span data-ttu-id="3eff5-162">Hej **aktivitet resurstypen** filter kan du tooselect en av följande hello filtrerar:</span><span class="sxs-lookup"><span data-stu-id="3eff5-162">hello **activity resource type** filter enables you tooselect one of hello following filters:</span></span>

- <span data-ttu-id="3eff5-163">Alla</span><span class="sxs-lookup"><span data-stu-id="3eff5-163">All</span></span> 
- <span data-ttu-id="3eff5-164">Grupp</span><span class="sxs-lookup"><span data-stu-id="3eff5-164">Group</span></span>
- <span data-ttu-id="3eff5-165">Katalog</span><span class="sxs-lookup"><span data-stu-id="3eff5-165">Directory</span></span>
- <span data-ttu-id="3eff5-166">Användare</span><span class="sxs-lookup"><span data-stu-id="3eff5-166">User</span></span>
- <span data-ttu-id="3eff5-167">Program</span><span class="sxs-lookup"><span data-stu-id="3eff5-167">Application</span></span>
- <span data-ttu-id="3eff5-168">Princip</span><span class="sxs-lookup"><span data-stu-id="3eff5-168">Policy</span></span>
- <span data-ttu-id="3eff5-169">Enhet</span><span class="sxs-lookup"><span data-stu-id="3eff5-169">Device</span></span>
- <span data-ttu-id="3eff5-170">Annat</span><span class="sxs-lookup"><span data-stu-id="3eff5-170">Other</span></span>

<span data-ttu-id="3eff5-171">När du väljer **grupp** som **aktivitet resurstypen**, får du en ytterligare filterkategori som du kan använda tooalso ger en **källa**:</span><span class="sxs-lookup"><span data-stu-id="3eff5-171">When you select **Group** as **activity resource type**, you get an additional filter category that enables you tooalso provide a **Source**:</span></span>

- <span data-ttu-id="3eff5-172">Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eff5-172">Azure AD</span></span>
- <span data-ttu-id="3eff5-173">O365</span><span class="sxs-lookup"><span data-stu-id="3eff5-173">O365</span></span>


<span data-ttu-id="3eff5-174">Hej **aktiviteten** filter baseras på hello kategori och aktivitet resurs typ du väljer.</span><span class="sxs-lookup"><span data-stu-id="3eff5-174">hello **activity** filter is based on hello category and Activity resource type selection you make.</span></span> <span data-ttu-id="3eff5-175">Du kan välja en specifik aktivitet som du vill toosee eller Välj alla.</span><span class="sxs-lookup"><span data-stu-id="3eff5-175">You can select a specific activity you want toosee or choose all.</span></span> 

<span data-ttu-id="3eff5-176">Du får hello lista över alla gransknings-och aktiviteter med hjälp av hello Graph API https://graph.windows.net/$ tenantdomain/aktiviteter/auditActivityTypes? api-version = beta, där $tenantdomain = ditt domännamn eller referera toohello artikel [granska rapporten händelser](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="3eff5-176">You can get hello list of all Audit Activities using hello Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, where $tenantdomain = your domain name or refer toohello article [audit report events](active-directory-reporting-audit-events.md).</span></span>


## <a name="audit-logs-shortcuts"></a><span data-ttu-id="3eff5-177">Genvägar till granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="3eff5-177">Audit logs shortcuts</span></span>

<span data-ttu-id="3eff5-178">Dessutom för**Azure Active Directory**, hello Azure-portalen ger dig två ytterligare startpunkter tooaudit data:</span><span class="sxs-lookup"><span data-stu-id="3eff5-178">In addition too**Azure Active Directory**, hello Azure portal provides you with two additional entry points tooaudit data:</span></span>

- <span data-ttu-id="3eff5-179">Användare och grupper</span><span class="sxs-lookup"><span data-stu-id="3eff5-179">Users and groups</span></span>
- <span data-ttu-id="3eff5-180">Företagsprogram</span><span class="sxs-lookup"><span data-stu-id="3eff5-180">Enterprise applications</span></span>

### <a name="users-and-groups-audit-logs"></a><span data-ttu-id="3eff5-181">Granskningsloggar för användare och grupper</span><span class="sxs-lookup"><span data-stu-id="3eff5-181">Users and groups audit logs</span></span>

<span data-ttu-id="3eff5-182">Du kan få svar tooquestions som med användar- och gruppbaserade granskningsrapporter:</span><span class="sxs-lookup"><span data-stu-id="3eff5-182">With user and group-based audit reports, you can get answers tooquestions such as:</span></span>

- <span data-ttu-id="3eff5-183">Vilka typer av uppdateringar har tillämpade hello användare?</span><span class="sxs-lookup"><span data-stu-id="3eff5-183">What types of updates have been applied hello users?</span></span>

- <span data-ttu-id="3eff5-184">Hur många användare ändrades?</span><span class="sxs-lookup"><span data-stu-id="3eff5-184">How many users were changed?</span></span>

- <span data-ttu-id="3eff5-185">Hur många lösenord ändrades?</span><span class="sxs-lookup"><span data-stu-id="3eff5-185">How many passwords were changed?</span></span>

- <span data-ttu-id="3eff5-186">Vad har en administratör gjort i en katalog?</span><span class="sxs-lookup"><span data-stu-id="3eff5-186">What has an administrator done in a directory?</span></span>

- <span data-ttu-id="3eff5-187">Vad är hello-grupper som har lagts till?</span><span class="sxs-lookup"><span data-stu-id="3eff5-187">What are hello groups that have been added?</span></span>

- <span data-ttu-id="3eff5-188">Finns det grupper med ändringar i medlemskap?</span><span class="sxs-lookup"><span data-stu-id="3eff5-188">Are there groups with membership changes?</span></span>

- <span data-ttu-id="3eff5-189">Har ändrats hello ägare av gruppen?</span><span class="sxs-lookup"><span data-stu-id="3eff5-189">Have hello owners of group been changed?</span></span>

- <span data-ttu-id="3eff5-190">Vilka licenser har tilldelats tooa grupp eller en användare?</span><span class="sxs-lookup"><span data-stu-id="3eff5-190">What licenses have been assigned tooa group or a user?</span></span>

<span data-ttu-id="3eff5-191">Om du bara vill tooreview granska data som är relaterade toousers och grupper du kan hitta en filtrerad vy under **granskningsloggar** i hello **aktiviteten** avsnitt i hello **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3eff5-191">If you just want tooreview auditing data that is related toousers and groups, you can find a filtered view under **Audit logs** in hello **Activity** section of hello **Users and Groups**.</span></span> <span data-ttu-id="3eff5-192">I det här området är **Användare och grupper** den förvalda **aktivitetsresurstypen**.</span><span class="sxs-lookup"><span data-stu-id="3eff5-192">This entry point has **Users and groups** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="3eff5-193">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/93.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-193">![Audit logs](./media/active-directory-reporting-activity-audit-logs/93.png "Audit logs")</span></span>

### <a name="enterprise-applications-audit-logs"></a><span data-ttu-id="3eff5-194">Granskningsloggar för företagsprogram</span><span class="sxs-lookup"><span data-stu-id="3eff5-194">Enterprise applications audit logs</span></span>

<span data-ttu-id="3eff5-195">Granska rapporter med programbaserade, du får svar tooquestions som:</span><span class="sxs-lookup"><span data-stu-id="3eff5-195">With application-based audit reports, you can get answers tooquestions such as:</span></span>

* <span data-ttu-id="3eff5-196">Vad är hello-program som har lagts till eller uppdaterats?</span><span class="sxs-lookup"><span data-stu-id="3eff5-196">What are hello applications that have been added or updated?</span></span>
* <span data-ttu-id="3eff5-197">Vad är hello-program som har tagits bort?</span><span class="sxs-lookup"><span data-stu-id="3eff5-197">What are hello applications that have been removed?</span></span>
* <span data-ttu-id="3eff5-198">Har tjänstprincipen för ett program ändrats?</span><span class="sxs-lookup"><span data-stu-id="3eff5-198">Has a service principle for an application changed?</span></span>
* <span data-ttu-id="3eff5-199">Har ändrats hello namnen på program?</span><span class="sxs-lookup"><span data-stu-id="3eff5-199">Have hello names of applications been changed?</span></span>
* <span data-ttu-id="3eff5-200">Vem som gav medgivande tooan program?</span><span class="sxs-lookup"><span data-stu-id="3eff5-200">Who gave consent tooan application?</span></span>

<span data-ttu-id="3eff5-201">Om du bara vill tooreview granska data som är relaterade tooyour program kan du hitta en filtrerad vy under **granskningsloggar** i hello **aktiviteten** avsnitt i hello **företagsprogram**  bladet.</span><span class="sxs-lookup"><span data-stu-id="3eff5-201">If you just want tooreview auditing data that is related tooyour applications, you can find a filtered view under **Audit logs** in hello **Activity** section of hello **Enterprise applications** blade.</span></span> <span data-ttu-id="3eff5-202">I det här området är **Företagsprogram** den förvalda **aktivitetsresurstypen**.</span><span class="sxs-lookup"><span data-stu-id="3eff5-202">This entry point has **Enterprise applications** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="3eff5-203">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/134.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-203">![Audit logs](./media/active-directory-reporting-activity-audit-logs/134.png "Audit logs")</span></span>

<span data-ttu-id="3eff5-204">Du kan filtrera den här vyn ytterligare ned toojust **grupper** eller bara **användare**.</span><span class="sxs-lookup"><span data-stu-id="3eff5-204">You can filter this view further down toojust **groups** or just **users**.</span></span>

<span data-ttu-id="3eff5-205">![Granskningsloggar](./media/active-directory-reporting-activity-audit-logs/25.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="3eff5-205">![Audit logs](./media/active-directory-reporting-activity-audit-logs/25.png "Audit logs")</span></span>


## <a name="next-steps"></a><span data-ttu-id="3eff5-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3eff5-206">Next steps</span></span>

<span data-ttu-id="3eff5-207">En översikt över rapportering finns i hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3eff5-207">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>

