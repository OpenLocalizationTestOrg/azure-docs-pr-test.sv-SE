---
title: aaaSign i aktivitetsrapporter hello Azure Active Directory-portalen | Microsoft Docs
description: Introduktion toosign i aktivitetsrapporter hello Azure Active Directory-portalen
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 49590d625a08d7dc189a629b89bab2261c2b4780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-reports-in-hello-azure-active-directory-portal"></a><span data-ttu-id="aa43a-103">Inloggningsaktivitet rapporter i hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="aa43a-103">Sign-in activity reports in hello Azure Active Directory portal</span></span>

<span data-ttu-id="aa43a-104">Med Azure Active Directory (AD Azure) rapportering i hello [Azure-portalen](https://portal.azure.com), kan du få hello information du behöver toodetermine hur din miljö gör.</span><span class="sxs-lookup"><span data-stu-id="aa43a-104">With Azure Active Directory (Azure AD) reporting in hello [Azure portal](https://portal.azure.com), you can get hello information you need toodetermine how your environment is doing.</span></span>

<span data-ttu-id="aa43a-105">hello-arkitekturen i Azure Active Directory reporting består av hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="aa43a-105">hello reporting architecture in Azure Active Directory consists of hello following components:</span></span>

- <span data-ttu-id="aa43a-106">**Aktivitet**</span><span class="sxs-lookup"><span data-stu-id="aa43a-106">**Activity**</span></span> 
    - <span data-ttu-id="aa43a-107">**Logga in aktiviteter** – Information om hello användning av hanterade program och användaren loggar in aktiviteter</span><span class="sxs-lookup"><span data-stu-id="aa43a-107">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="aa43a-108">**Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="aa43a-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="aa43a-109">**Säkerhet**</span><span class="sxs-lookup"><span data-stu-id="aa43a-109">**Security**</span></span> 
    - <span data-ttu-id="aa43a-110">**Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="aa43a-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="aa43a-111">Mer information finns i avsnittet om riskfyllda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="aa43a-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="aa43a-112">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="aa43a-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="aa43a-113">Mer information finns i avsnittet om användare som har flaggats för risk.</span><span class="sxs-lookup"><span data-stu-id="aa43a-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="aa43a-114">Det här avsnittet ger en översikt över hello inloggning aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="aa43a-114">This topic gives you an overview of hello sign-in activities.</span></span>

## <a name="pre-requisite"></a><span data-ttu-id="aa43a-115">Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="aa43a-115">Pre-requisite</span></span>

### <a name="who-can-access-hello-data"></a><span data-ttu-id="aa43a-116">Vem som kan komma åt hello data?</span><span class="sxs-lookup"><span data-stu-id="aa43a-116">Who can access hello data?</span></span>
* <span data-ttu-id="aa43a-117">Användare med hello säkerhet Admin eller säkerhet Reader rollen</span><span class="sxs-lookup"><span data-stu-id="aa43a-117">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="aa43a-118">Globala administratörer</span><span class="sxs-lookup"><span data-stu-id="aa43a-118">Global Admins</span></span>
* <span data-ttu-id="aa43a-119">Alla användare (icke-administratörer) kan komma åt sina egna inloggningar</span><span class="sxs-lookup"><span data-stu-id="aa43a-119">Any user (non-admins) can access their own sign-ins</span></span> 

### <a name="what-azure-ad-license-do-you-need-tooaccess-sign-in-activity"></a><span data-ttu-id="aa43a-120">Vilka Azure AD-licens behöver du tooaccess inloggningsaktivitet?</span><span class="sxs-lookup"><span data-stu-id="aa43a-120">What Azure AD license do you need tooaccess sign-in activity?</span></span>
* <span data-ttu-id="aa43a-121">Din klient måste ha en Azure AD Premium-licens som är associerade med den toosee hello alla upp inloggningsaktivitet rapport</span><span class="sxs-lookup"><span data-stu-id="aa43a-121">Your tenant must have an Azure AD Premium license associated with it toosee hello all up sign-in activity report</span></span>


## <a name="signs-in-activities"></a><span data-ttu-id="aa43a-122">Inloggningsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="aa43a-122">Signs-in activities</span></span>

<span data-ttu-id="aa43a-123">Hello information som tillhandahålls av hello användaren logga in rapporten, kan du hitta svar tooquestions som:</span><span class="sxs-lookup"><span data-stu-id="aa43a-123">With hello information provided by hello user sign-in report, you find answers tooquestions such as:</span></span>

* <span data-ttu-id="aa43a-124">Vad är hello inloggning mönstret för en användare?</span><span class="sxs-lookup"><span data-stu-id="aa43a-124">What is hello sign-in pattern of a user?</span></span>
* <span data-ttu-id="aa43a-125">Hur många användare har en användare loggat in under en vecka?</span><span class="sxs-lookup"><span data-stu-id="aa43a-125">How many users have users signed in over a week?</span></span>
* <span data-ttu-id="aa43a-126">Vad är hello statusen för dessa inloggningar?</span><span class="sxs-lookup"><span data-stu-id="aa43a-126">What’s hello status of these sign-ins?</span></span>

<span data-ttu-id="aa43a-127">Dina första posten punkt tooall inloggning aktiviteter data är **inloggningar** under hello aktivitet i **Azure Active**.</span><span class="sxs-lookup"><span data-stu-id="aa43a-127">Your first entry point tooall sign-in activities data is **Sign-ins** in hello Activity section of **Azure Active**.</span></span>


<span data-ttu-id="aa43a-128">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/61.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-128">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/61.png "Sign-in activity")</span></span>


<span data-ttu-id="aa43a-129">En granskningslogg har en standardlistvy som visar:</span><span class="sxs-lookup"><span data-stu-id="aa43a-129">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="aa43a-130">hello relaterade användare</span><span class="sxs-lookup"><span data-stu-id="aa43a-130">hello related user</span></span>
- <span data-ttu-id="aa43a-131">hello hello programanvändare har inloggad till</span><span class="sxs-lookup"><span data-stu-id="aa43a-131">hello application hello user has signed-in to</span></span>
- <span data-ttu-id="aa43a-132">Hej inloggningsstatusen</span><span class="sxs-lookup"><span data-stu-id="aa43a-132">hello sign-in status</span></span>
- <span data-ttu-id="aa43a-133">hello inloggning tid</span><span class="sxs-lookup"><span data-stu-id="aa43a-133">hello sign-in time</span></span>

<span data-ttu-id="aa43a-134">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/41.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-134">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-135">Du kan anpassa hello listvyn genom att klicka på **kolumner** i hello-verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="aa43a-135">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>

<span data-ttu-id="aa43a-136">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/19.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-136">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/19.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-137">Detta gör att du toodisplay ytterligare fält eller ta bort fält som redan visas.</span><span class="sxs-lookup"><span data-stu-id="aa43a-137">This enables you toodisplay additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="aa43a-138">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/42.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-138">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/42.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-139">Genom att klicka på ett objekt i listvyn hello, hämta alla information om den.</span><span class="sxs-lookup"><span data-stu-id="aa43a-139">By clicking an item in hello list view, you get all available details about it.</span></span>

<span data-ttu-id="aa43a-140">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/43.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-140">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/43.png "Sign-in activity")</span></span>


## <a name="filtering-sign-in-activities"></a><span data-ttu-id="aa43a-141">Filtrerar inloggningsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="aa43a-141">Filtering sign-in activities</span></span>

<span data-ttu-id="aa43a-142">toonarrow ned hello rapporterade data tooa nivå som fungerar för dig, kan du filtrera hello inloggningar data med hjälp av hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="aa43a-142">toonarrow down hello reported data tooa level that works for you, you can filter hello sign-ins data using hello following fields:</span></span>

- <span data-ttu-id="aa43a-143">Tidsintervall</span><span class="sxs-lookup"><span data-stu-id="aa43a-143">Time interval</span></span>
- <span data-ttu-id="aa43a-144">Användare</span><span class="sxs-lookup"><span data-stu-id="aa43a-144">User</span></span>
- <span data-ttu-id="aa43a-145">Program</span><span class="sxs-lookup"><span data-stu-id="aa43a-145">Application</span></span>
- <span data-ttu-id="aa43a-146">Client</span><span class="sxs-lookup"><span data-stu-id="aa43a-146">Client</span></span>
- <span data-ttu-id="aa43a-147">Inloggningsstatus</span><span class="sxs-lookup"><span data-stu-id="aa43a-147">Sign-in status</span></span>

<span data-ttu-id="aa43a-148">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/44.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-148">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/44.png "Sign-in activity")</span></span>


<span data-ttu-id="aa43a-149">Hej **tidsintervall** filter aktiverar tooyou toodefine en tidsram för hello returnerade data.</span><span class="sxs-lookup"><span data-stu-id="aa43a-149">hello **time interval** filter enables tooyou toodefine a timeframe for hello returned data.</span></span>  
<span data-ttu-id="aa43a-150">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="aa43a-150">Possible values are:</span></span>

- <span data-ttu-id="aa43a-151">1 månad</span><span class="sxs-lookup"><span data-stu-id="aa43a-151">1 month</span></span>
- <span data-ttu-id="aa43a-152">7 dagar</span><span class="sxs-lookup"><span data-stu-id="aa43a-152">7 days</span></span>
- <span data-ttu-id="aa43a-153">24 timmar</span><span class="sxs-lookup"><span data-stu-id="aa43a-153">24 hours</span></span>
- <span data-ttu-id="aa43a-154">Anpassat</span><span class="sxs-lookup"><span data-stu-id="aa43a-154">Custom</span></span>

<span data-ttu-id="aa43a-155">När du väljer en anpassad tidsram kan du konfigurera en starttid och en sluttid.</span><span class="sxs-lookup"><span data-stu-id="aa43a-155">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="aa43a-156">Hej **användaren** filter kan du toospecify hello namn eller hello användarens huvudnamn (UPN) för hello användare som intresserar dig.</span><span class="sxs-lookup"><span data-stu-id="aa43a-156">hello **user** filter enables you toospecify hello name or hello user principal name (UPN) of hello user you care about.</span></span>

<span data-ttu-id="aa43a-157">Hej **programmet** filter kan du toospecify hello namnet på hello-program som intresserar dig.</span><span class="sxs-lookup"><span data-stu-id="aa43a-157">hello **application** filter enables you toospecify hello name of hello application you care about.</span></span>

<span data-ttu-id="aa43a-158">Hej **klienten** filter kan du toospecify information om hello-enhet som intresserar dig.</span><span class="sxs-lookup"><span data-stu-id="aa43a-158">hello **client** filter enables you toospecify information about hello device you care about.</span></span>

<span data-ttu-id="aa43a-159">Hej **inloggningsstatusen** filter kan du tooselect något av följande filter hello:</span><span class="sxs-lookup"><span data-stu-id="aa43a-159">hello **sign-in status** filter enables you tooselect one of hello following filter:</span></span>

- <span data-ttu-id="aa43a-160">Alla</span><span class="sxs-lookup"><span data-stu-id="aa43a-160">All</span></span>
- <span data-ttu-id="aa43a-161">Lyckades</span><span class="sxs-lookup"><span data-stu-id="aa43a-161">Success</span></span>
- <span data-ttu-id="aa43a-162">Fel</span><span class="sxs-lookup"><span data-stu-id="aa43a-162">Failure</span></span>


## <a name="sign-in-activities-shortcuts"></a><span data-ttu-id="aa43a-163">Genvägar till inloggningsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="aa43a-163">Sign-in activities shortcuts</span></span>

<span data-ttu-id="aa43a-164">Dessutom tooAzure Active Directory hello Azure-portalen ger dig två ytterligare posten pekar toosign i aktiviteter data:</span><span class="sxs-lookup"><span data-stu-id="aa43a-164">In addition tooAzure Active Directory, hello Azure portal provides you with two additional entry points toosign-in activities data:</span></span>

- <span data-ttu-id="aa43a-165">Användare och grupper</span><span class="sxs-lookup"><span data-stu-id="aa43a-165">Users and groups</span></span>
- <span data-ttu-id="aa43a-166">Företagsprogram</span><span class="sxs-lookup"><span data-stu-id="aa43a-166">Enterprise applications</span></span>


### <a name="users-and-groups-sign-ins-activities"></a><span data-ttu-id="aa43a-167">Inloggningsaktivitet för användare och grupper</span><span class="sxs-lookup"><span data-stu-id="aa43a-167">Users and groups sign-ins activities</span></span>

<span data-ttu-id="aa43a-168">Hello information som tillhandahålls av hello användaren logga in rapporten, kan du hitta svar tooquestions som:</span><span class="sxs-lookup"><span data-stu-id="aa43a-168">With hello information provided by hello user sign-in report, you find answers tooquestions such as:</span></span>

- <span data-ttu-id="aa43a-169">Vad är hello inloggning mönstret för en användare?</span><span class="sxs-lookup"><span data-stu-id="aa43a-169">What is hello sign-in pattern of a user?</span></span>
- <span data-ttu-id="aa43a-170">Hur många användare har en användare loggat in under en vecka?</span><span class="sxs-lookup"><span data-stu-id="aa43a-170">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="aa43a-171">Vad är hello statusen för dessa inloggningar?</span><span class="sxs-lookup"><span data-stu-id="aa43a-171">What’s hello status of these sign-ins?</span></span>



<span data-ttu-id="aa43a-172">Punkt toothis transaktionsdata är hello användaren logga in diagrammet i hello **översikt** avsnittet **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="aa43a-172">Your entry point toothis data is hello user sign-in graph in hello **Overview** section under **Users and groups**.</span></span>

<span data-ttu-id="aa43a-173">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/45.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-173">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/45.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-174">hello användaren logga in diagrammet visar veckovisa aggregeringar för logga moduler för alla användare i en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="aa43a-174">hello user sign-in graph shows weekly aggregations of sign ins for all users in a given time period.</span></span> <span data-ttu-id="aa43a-175">hello standardvärdet för hello är tidsperiod 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="aa43a-175">hello default for hello time period is 30 days.</span></span>

<span data-ttu-id="aa43a-176">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/46.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-176">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/46.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-177">När du klickar på en dag i hello inloggning graph få en detaljerad lista över hello inloggning aktiviteter för den aktuella dagen.</span><span class="sxs-lookup"><span data-stu-id="aa43a-177">When you click on a day in hello sign-in graph, you get a detailed list of hello sign-in activities for this day.</span></span>

<span data-ttu-id="aa43a-178">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/41.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-178">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-179">Varje rad i hello inloggning aktiviteter lista ger du hello detaljerad information om hello valt logga in som:</span><span class="sxs-lookup"><span data-stu-id="aa43a-179">Each row in hello sign-in activities list gives you hello detailed information about hello selected sign-in such as:</span></span>

* <span data-ttu-id="aa43a-180">Vem har loggat in?</span><span class="sxs-lookup"><span data-stu-id="aa43a-180">Who has signed in?</span></span>
* <span data-ttu-id="aa43a-181">Vad hette hello relaterade UPN?</span><span class="sxs-lookup"><span data-stu-id="aa43a-181">What was hello related UPN?</span></span>
* <span data-ttu-id="aa43a-182">Vilka program har hello målet för inloggning hello?</span><span class="sxs-lookup"><span data-stu-id="aa43a-182">What application was hello target of hello sign-in?</span></span>
* <span data-ttu-id="aa43a-183">Vad är hello IP-adressen för inloggning hello?</span><span class="sxs-lookup"><span data-stu-id="aa43a-183">What is hello IP address of hello sign-in?</span></span>
* <span data-ttu-id="aa43a-184">Vad hette hello status för inloggning hello?</span><span class="sxs-lookup"><span data-stu-id="aa43a-184">What was hello status of hello sign-in?</span></span>

<span data-ttu-id="aa43a-185">Hej **inloggningar** alternativet ger en fullständig överblick över alla användarinloggningar.</span><span class="sxs-lookup"><span data-stu-id="aa43a-185">hello **Sign-ins** option gives you a complete overview of all user sign-ins.</span></span>

<span data-ttu-id="aa43a-186">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/51.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-186">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/51.png "Sign-in activity")</span></span>



## <a name="usage-of-managed-applications"></a><span data-ttu-id="aa43a-187">Användning av hanterade program</span><span class="sxs-lookup"><span data-stu-id="aa43a-187">Usage of managed applications</span></span>

<span data-ttu-id="aa43a-188">Med en programcentrerad vy över dina inloggningsuppgifter kan du få svar på frågor som:</span><span class="sxs-lookup"><span data-stu-id="aa43a-188">With an application-centric view of your sign-in data, you can answer questions such as:</span></span>

* <span data-ttu-id="aa43a-189">Vem använder mina program?</span><span class="sxs-lookup"><span data-stu-id="aa43a-189">Who is using my applications?</span></span>
* <span data-ttu-id="aa43a-190">Vad är hello översta 3-program i din organisation?</span><span class="sxs-lookup"><span data-stu-id="aa43a-190">What are hello top 3 applications in your organization?</span></span>
* <span data-ttu-id="aa43a-191">Jag har nyligen distribuerat ett program.</span><span class="sxs-lookup"><span data-stu-id="aa43a-191">I have recently rolled out an application.</span></span> <span data-ttu-id="aa43a-192">Hur går det för det?</span><span class="sxs-lookup"><span data-stu-id="aa43a-192">How is it doing?</span></span>

<span data-ttu-id="aa43a-193">Punkt toothis transaktionsdata är hello översta 3-program i din organisation i hello senaste 30 dagarna rapporten i hello **översikt** avsnittet **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="aa43a-193">Your entry point toothis data is hello top 3 applications in your organization within hello last 30 days report in hello **Overview** section under **Enterprise applications**.</span></span>

<span data-ttu-id="aa43a-194">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/64.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-194">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/64.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-195">hello app användning diagrammet veckovisa aggregeringar för inloggningar för översta 3-program i en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="aa43a-195">hello app usage graph weekly aggregations of sign ins for your top 3 applications in a given time period.</span></span> <span data-ttu-id="aa43a-196">hello standardvärdet för hello är tidsperiod 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="aa43a-196">hello default for hello time period is 30 days.</span></span>

<span data-ttu-id="aa43a-197">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/47.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-197">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/47.png "Sign-in activity")</span></span>

<span data-ttu-id="aa43a-198">Om du vill kan ange du hello fokus på ett visst program.</span><span class="sxs-lookup"><span data-stu-id="aa43a-198">If you want to, you can set hello focus on a specific application.</span></span>


<span data-ttu-id="aa43a-199">![Rapportering](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Rapportering")</span><span class="sxs-lookup"><span data-stu-id="aa43a-199">![Reporting](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")</span></span>

<span data-ttu-id="aa43a-200">När du klickar på en dag i hello app Användningsdiagram få en detaljerad lista över hello inloggning aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="aa43a-200">When you click on a day in hello app usage graph, you get a detailed list of hello sign-in activities.</span></span>


<span data-ttu-id="aa43a-201">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/48.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-201">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/48.png "Sign-in activity")</span></span>


<span data-ttu-id="aa43a-202">Hej **inloggningar** alternativet ger en fullständig överblick över alla inloggning händelser tooyour program.</span><span class="sxs-lookup"><span data-stu-id="aa43a-202">hello **Sign-ins** option gives you a complete overview of all sign-in events tooyour applications.</span></span>

<span data-ttu-id="aa43a-203">![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins/49.png "Inloggningsaktivitet")</span><span class="sxs-lookup"><span data-stu-id="aa43a-203">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/49.png "Sign-in activity")</span></span>



## <a name="next-steps"></a><span data-ttu-id="aa43a-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa43a-204">Next steps</span></span>

<span data-ttu-id="aa43a-205">Om du vill tooknow mer om inloggningsaktivitet felkoder finns hello [inloggning aktivitet rapporten felkoder i hello Azure Active Directory-portalen](active-directory-reporting-activity-sign-ins-errors.md).</span><span class="sxs-lookup"><span data-stu-id="aa43a-205">If you want tooknow more about sign-in activity error codes, see hello [Sign-in activity report error codes in hello Azure Active Directory portal](active-directory-reporting-activity-sign-ins-errors.md).</span></span>

