---
title: aaaFind aktivitetsrapporter i hello Azure-portalen | Microsoft Docs
description: "Lär dig hur toofind Azure Active Directory-aktivitet rapporter i hello Azure-portalen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f8d7a968403e10ccc5319f27fedad38b1553ded0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-activity-reports-in-hello-azure-portal"></a><span data-ttu-id="1e4a6-103">Hitta aktivitetsrapporter i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e4a6-103">Find activity reports in hello Azure portal</span></span>

<span data-ttu-id="1e4a6-104">Om du flyttar från hello Azure klassiska portal toohello Azure-portalen, får du en ny titt på Azure Active Directory (AD Azure) aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-104">If you are moving from hello Azure classic portal toohello Azure portal, you get a new look at Azure Active Directory (Azure AD) activity logs.</span></span> <span data-ttu-id="1e4a6-105">I en senaste [blogginlägget](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), förklarar vi hur du kan se aktiviteten loggar hello gäller hello-resurs som du arbetar med i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-105">In a recent [blog post](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), we explain how you can see activity logs in hello context of hello resource you are working on in hello Azure portal.</span></span> <span data-ttu-id="1e4a6-106">I den här artikeln beskriver vi hur toofind rapporter som du använde i hello klassiska Azure-portalen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-106">In this article, we describe how toofind reports that you used in hello Azure classic portal in hello Azure portal.</span></span>

## <a name="whats-new"></a><span data-ttu-id="1e4a6-107">Nyheter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-107">What's new</span></span>

<span data-ttu-id="1e4a6-108">Rapporter i hello klassiska Azure-portalen är uppdelade i kategorier:</span><span class="sxs-lookup"><span data-stu-id="1e4a6-108">Reports in hello Azure classic portal are separated into categories:</span></span>

1.  <span data-ttu-id="1e4a6-109">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-109">Security reports</span></span>
2.  <span data-ttu-id="1e4a6-110">Aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-110">Activity reports</span></span>
3.  <span data-ttu-id="1e4a6-111">Integrerad apprapporter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-111">Integrated app reports</span></span>

### <a name="activity-and-integrated-app-reports"></a><span data-ttu-id="1e4a6-112">Aktiviteten och integrerad apprapporter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-112">Activity and integrated app reports</span></span>

<span data-ttu-id="1e4a6-113">För sammanhangsberoende rapportering i hello Azure-portalen, kombineras befintliga rapporter till en enda vy.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-113">For context-based reporting in hello Azure portal, existing reports are merged into a single view.</span></span> <span data-ttu-id="1e4a6-114">En enda, underliggande API ger hello toohello datavy.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-114">A single, underlying API provides hello data toohello view.</span></span>

<span data-ttu-id="1e4a6-115">Detta visas på hello toosee **Azure Active Directory** bladet under **AKTIVITETEN**väljer **granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-115">toosee this view, on hello **Azure Active Directory** blade, under **ACTIVITY**, select **Audit logs**.</span></span>

<span data-ttu-id="1e4a6-116">![Granskningsloggar](./media/active-directory-reporting-migration/482.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-116">![Audit logs](./media/active-directory-reporting-migration/482.png "Audit logs")</span></span>

<span data-ttu-id="1e4a6-117">följande rapporter hello konsolideras i den här vyn:</span><span class="sxs-lookup"><span data-stu-id="1e4a6-117">hello following reports are consolidated in this view:</span></span>

-   <span data-ttu-id="1e4a6-118">Granska rapporten</span><span class="sxs-lookup"><span data-stu-id="1e4a6-118">Audit report</span></span>
-   <span data-ttu-id="1e4a6-119">Lösenordsåterställningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-119">Password reset activity</span></span>
-   <span data-ttu-id="1e4a6-120">Lösenordsåterställningsaktivitet för registrering</span><span class="sxs-lookup"><span data-stu-id="1e4a6-120">Password reset registration activity</span></span>
-   <span data-ttu-id="1e4a6-121">Självbetjäning grupper aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-121">Self-service groups activity</span></span>
-   <span data-ttu-id="1e4a6-122">Ändringar av Office365 namn</span><span class="sxs-lookup"><span data-stu-id="1e4a6-122">Office365 Group Name Changes</span></span>
-   <span data-ttu-id="1e4a6-123">Kontot etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-123">Account provisioning activity</span></span>
-   <span data-ttu-id="1e4a6-124">Status för förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="1e4a6-124">Password rollover status</span></span>
-   <span data-ttu-id="1e4a6-125">Kontoetableringsfel</span><span class="sxs-lookup"><span data-stu-id="1e4a6-125">Account provisioning errors</span></span>


<span data-ttu-id="1e4a6-126">hello programanvändning rapporten har förbättrats och ingår i hello **inloggningar** vyn.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-126">hello Application Usage report has been enhanced and is included in hello **Sign-ins** view.</span></span> <span data-ttu-id="1e4a6-127">Detta visas på hello toosee **Azure Active Directory** bladet under **AKTIVITETEN**väljer **inloggningar**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-127">toosee this view, on hello **Azure Active Directory** blade, under **ACTIVITY**, select **Sign-ins**.</span></span>

<span data-ttu-id="1e4a6-128">![Inloggningar visa](./media/active-directory-reporting-migration/483.png "inloggningar visa")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-128">![Sign-ins view](./media/active-directory-reporting-migration/483.png "Sign-ins view")</span></span>

<span data-ttu-id="1e4a6-129">Hej **inloggningar** vyn innehåller alla användarinloggningar. Du kan använda denna information tooget användning programinformation.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-129">hello **Sign-ins** view includes all user sign-ins. You can use this information tooget application usage information.</span></span> <span data-ttu-id="1e4a6-130">Du kan också visa information om användning i hello **företagsprogram** översikt i hello **hantera** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-130">You also can view application usage information in hello **Enterprise applications** overview, in hello **MANAGE** section.</span></span>

<span data-ttu-id="1e4a6-131">![Företagsprogram](./media/active-directory-reporting-migration/484.png "företagsprogram")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-131">![Enterprise applications](./media/active-directory-reporting-migration/484.png "Enterprise applications")</span></span>

## <a name="access-a-specific-report"></a><span data-ttu-id="1e4a6-132">Åtkomst till en rapport</span><span class="sxs-lookup"><span data-stu-id="1e4a6-132">Access a specific report</span></span>

<span data-ttu-id="1e4a6-133">Även om hello Azure-portalen erbjuder en enda vy kan titta du också på specifika rapporter.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-133">Although hello Azure portal offers a single view, you also can look at specific reports.</span></span>

### <a name="audit-logs"></a><span data-ttu-id="1e4a6-134">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="1e4a6-134">Audit logs</span></span>

<span data-ttu-id="1e4a6-135">I svaret toocustomer feedback kan i hello Azure-portalen, du använda avancerad filtrering tooaccess hello data som du vill.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-135">In response toocustomer feedback, in hello Azure portal, you can use advanced filtering tooaccess hello data you want.</span></span> <span data-ttu-id="1e4a6-136">Är ett filter som du kan använda en *aktivitetskategorin*, som visar hello olika typer av aktiviteten loggar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-136">One filter you can use is an *activity category*, which lists hello different types of activity logs in Azure AD.</span></span> <span data-ttu-id="1e4a6-137">toonarrow resulterar toowhat som du söker efter kan du välja en kategori.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-137">toonarrow results toowhat you are looking for, you can select a category.</span></span>

<span data-ttu-id="1e4a6-138">Om du är intresserad av att endast aktiviteter relaterade tooself service lösenordsåterställning kan du till exempel välja hello **Self-service lösenordshantering** kategori.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-138">For example, if you are interested only in activities related tooself-service password resets, you can choose hello **Self-service Password Management** category.</span></span> <span data-ttu-id="1e4a6-139">hello-kategorier som du ser baseras på hello-resurs som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-139">hello categories you see are based on hello resource you are working in.</span></span>  

<span data-ttu-id="1e4a6-140">![Kategori alternativen på hello Filter granskningsloggar sidan](./media/active-directory-reporting-migration/06.png "kategori alternativen på sidan för hello-Filter granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-140">![Category options on hello Filter Audit Logs page](./media/active-directory-reporting-migration/06.png "Category options on hello Filter Audit Logs page")</span></span>

<span data-ttu-id="1e4a6-141">Aktivitetskategorier inkluderar:</span><span class="sxs-lookup"><span data-stu-id="1e4a6-141">Activity categories include:</span></span>

- <span data-ttu-id="1e4a6-142">Kärnkatalog</span><span class="sxs-lookup"><span data-stu-id="1e4a6-142">Core Directory</span></span>
- <span data-ttu-id="1e4a6-143">Hantering av lösenord för självbetjäning</span><span class="sxs-lookup"><span data-stu-id="1e4a6-143">Self-service Password Management</span></span>
- <span data-ttu-id="1e4a6-144">Självbetjäning, grupphantering</span><span class="sxs-lookup"><span data-stu-id="1e4a6-144">Self-service Group Management</span></span>
- <span data-ttu-id="1e4a6-145">Kontoetablering</span><span class="sxs-lookup"><span data-stu-id="1e4a6-145">Account Provisioning</span></span>

### <a name="application-usage"></a><span data-ttu-id="1e4a6-146">Programanvändning</span><span class="sxs-lookup"><span data-stu-id="1e4a6-146">Application usage</span></span>

<span data-ttu-id="1e4a6-147">tooview information om programanvändning för alla appar eller för en enda app under **AKTIVITETEN**väljer **inloggningar**. toonarrow hello resultat kan du filtrera efter användarnamn eller programnamn.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-147">tooview details about application usage for all apps or for a single app, under **ACTIVITY**, select **Sign-ins**. toonarrow hello results, you can filter on user name or application name.</span></span>

<span data-ttu-id="1e4a6-148">![Filter inloggning händelser sidan](./media/active-directory-reporting-migration/07.png "filtrera inloggning händelser sida")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-148">![Filter Sign-In Events page](./media/active-directory-reporting-migration/07.png "Filter Sign-In Events page")</span></span>

### <a name="security-reports"></a><span data-ttu-id="1e4a6-149">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-149">Security reports</span></span>

#### <a name="azure-ad-anomalous-activity-reports"></a><span data-ttu-id="1e4a6-150">Azure AD avvikande aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-150">Azure AD anomalous activity reports</span></span>

<span data-ttu-id="1e4a6-151">Azure AD avvikande aktivitetssäkerhet rapporter från hello klassiska Azure-portalen har konsoliderade tooprovide du med en enda central vy.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-151">Azure AD anomalous activity security reports from hello Azure classic portal have been consolidated tooprovide you with one, central view.</span></span> <span data-ttu-id="1e4a6-152">Den här vyn visar alla riskhändelser som säkerhetsrelaterade att Azure AD kan identifiera och rapportera om.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-152">This view shows all security-related risk events that Azure AD can detect and report on.</span></span>

<span data-ttu-id="1e4a6-153">hello i den följande tabellen listas hello Azure AD avvikande aktivitet säkerhetsrapporter och motsvarande risk händelsetyper i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-153">hello following table lists hello Azure AD anomalous activity security reports, and corresponding risk event types in hello Azure portal.</span></span>

| <span data-ttu-id="1e4a6-154">Azure AD avvikande Aktivitetsrapport</span><span class="sxs-lookup"><span data-stu-id="1e4a6-154">Azure AD anomalous activity report</span></span> |  <span data-ttu-id="1e4a6-155">Identity protection risk händelsetyp</span><span class="sxs-lookup"><span data-stu-id="1e4a6-155">Identity protection risk event type</span></span>|
| :--- | :--- |
| <span data-ttu-id="1e4a6-156">Används med läckta autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-156">Users with leaked credentials</span></span> | <span data-ttu-id="1e4a6-157">Läckta autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-157">Leaked credentials</span></span> |
| <span data-ttu-id="1e4a6-158">Oregelbunden inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-158">Irregular sign-in activity</span></span> | <span data-ttu-id="1e4a6-159">Omöjlig resa tooatypical platser</span><span class="sxs-lookup"><span data-stu-id="1e4a6-159">Impossible travel tooatypical locations</span></span> |
| <span data-ttu-id="1e4a6-160">Inloggningar från potentiellt infekterade enheter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-160">Sign-ins from possibly infected devices</span></span> | <span data-ttu-id="1e4a6-161">Inloggningar från angripna enheter</span><span class="sxs-lookup"><span data-stu-id="1e4a6-161">Sign-ins from infected devices</span></span>|
| <span data-ttu-id="1e4a6-162">Inloggningar från okända källor</span><span class="sxs-lookup"><span data-stu-id="1e4a6-162">Sign-ins from unknown sources</span></span> | <span data-ttu-id="1e4a6-163">Inloggningar från anonyma IP-adresser</span><span class="sxs-lookup"><span data-stu-id="1e4a6-163">Sign-ins from anonymous IP addresses</span></span> |
| <span data-ttu-id="1e4a6-164">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-164">Sign-ins from IP addresses with suspicious activity</span></span> | <span data-ttu-id="1e4a6-165">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-165">Sign-ins from IP addresses with suspicious activity</span></span> |
| - | <span data-ttu-id="1e4a6-166">Inloggningar från okända platser</span><span class="sxs-lookup"><span data-stu-id="1e4a6-166">Sign-ins from unfamiliar locations</span></span> |

<span data-ttu-id="1e4a6-167">hello följande Azure AD avvikande aktivitetssäkerhet rapporterar inte ingår som riskhändelser i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="1e4a6-167">hello following Azure AD anomalous activity security reports are not included as risk events in hello Azure portal:</span></span>

* <span data-ttu-id="1e4a6-168">Inloggningar efter flera fel</span><span class="sxs-lookup"><span data-stu-id="1e4a6-168">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="1e4a6-169">Inloggningar från flera geografiska områden</span><span class="sxs-lookup"><span data-stu-id="1e4a6-169">Sign-ins from multiple geographies</span></span>

<span data-ttu-id="1e4a6-170">Dessa rapporter är kvar i hello klassiska Azure-portalen, men de kommer att inaktualiseras någon gång hello framtida.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-170">These reports are still available in hello Azure classic portal, but they will be deprecated at some time in hello future.</span></span>

<span data-ttu-id="1e4a6-171">Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="1e4a6-171">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>  


#### <a name="detected-risk-events"></a><span data-ttu-id="1e4a6-172">Identifierade riskhändelser</span><span class="sxs-lookup"><span data-stu-id="1e4a6-172">Detected risk events</span></span>

<span data-ttu-id="1e4a6-173">I hello Azure-portalen, kan du komma åt rapporter om identifierad riskhändelser på hello **Azure Active Directory** bladet under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-173">In hello Azure portal, you can access reports about detected risk events on hello **Azure Active Directory** blade, under **SECURITY**.</span></span> <span data-ttu-id="1e4a6-174">Identifierade riskhändelser spåras i hello följande rapporter:</span><span class="sxs-lookup"><span data-stu-id="1e4a6-174">Detected risk events are tracked in hello following reports:</span></span>   

- <span data-ttu-id="1e4a6-175">Användare i riskzonen</span><span class="sxs-lookup"><span data-stu-id="1e4a6-175">Users at Risk</span></span>
- <span data-ttu-id="1e4a6-176">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="1e4a6-176">Risky Sign-ins</span></span>

<span data-ttu-id="1e4a6-177">![Säkerhetsrapporter](./media/active-directory-reporting-migration/04.png "säkerhetsrapporter")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-177">![Security reports](./media/active-directory-reporting-migration/04.png "Security reports")</span></span>

<span data-ttu-id="1e4a6-178">Mer information om säkerhetsrapporter finns:</span><span class="sxs-lookup"><span data-stu-id="1e4a6-178">For more information about security reports, see:</span></span>

- [<span data-ttu-id="1e4a6-179">Användare på Risk säkerhetsrapporten hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="1e4a6-179">Users at Risk security report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="1e4a6-180">Riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="1e4a6-180">Risky Sign-ins report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-hello-azure-classic-portal-vs-hello-azure-portal"></a><span data-ttu-id="1e4a6-181">Aktivitetsrapporter i hello klassiska Azure-portalen kontra hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e4a6-181">Activity reports in hello Azure classic portal vs. hello Azure portal</span></span>

<span data-ttu-id="1e4a6-182">hello tabell i det här avsnittet innehåller befintliga rapporter i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-182">hello table in this section lists existing reports in hello Azure classic portal.</span></span> <span data-ttu-id="1e4a6-183">Här beskrivs också hur du kan få hello samma information i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-183">It also describes how you can get hello same information in hello Azure portal.</span></span>

<span data-ttu-id="1e4a6-184">alla tooview granska data på hello **Azure Active Directory** bladet under **AKTIVITETEN**, gå för**granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-184">tooview all auditing data, on hello **Azure Active Directory** blade, under **ACTIVITY**, go too**Audit logs**.</span></span>

<span data-ttu-id="1e4a6-185">![Granskningsloggar](./media/active-directory-reporting-migration/61.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-185">![Audit logs](./media/active-directory-reporting-migration/61.png "Audit logs")</span></span>

| <span data-ttu-id="1e4a6-186">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="1e4a6-186">Azure classic portal</span></span>                 | <span data-ttu-id="1e4a6-187">toofind i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e4a6-187">toofind in hello Azure portal</span></span>                                                         |
| ---                                  | ---                                                                        |
| <span data-ttu-id="1e4a6-188">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="1e4a6-188">Audit logs</span></span>                           | <span data-ttu-id="1e4a6-189">För **aktivitetskategorin**väljer **Core Directory**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-189">For **Activity Category**, select **Core Directory**.</span></span>                       |
| <span data-ttu-id="1e4a6-190">Lösenordsåterställningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-190">Password reset activity</span></span>              | <span data-ttu-id="1e4a6-191">För **aktivitetskategorin**väljer **Self-service lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-191">For **Activity Category**, select **Self-service Password Management**.</span></span> |
| <span data-ttu-id="1e4a6-192">Lösenordsåterställningsaktivitet för registrering</span><span class="sxs-lookup"><span data-stu-id="1e4a6-192">Password reset registration activity</span></span> | <span data-ttu-id="1e4a6-193">För **aktivitetskategorin**väljer **Self-service lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-193">For **Activity Category**, select **Self-service Password Management**.</span></span>     |
| <span data-ttu-id="1e4a6-194">Självbetjäning grupper aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-194">Self-service groups activity</span></span>         | <span data-ttu-id="1e4a6-195">För **aktivitetskategorin**väljer **Self-service Group Management**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-195">For **Activity Category**, select **Self-service Group Management**.</span></span>        |
| <span data-ttu-id="1e4a6-196">Kontot etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e4a6-196">Account provisioning activity</span></span>        | <span data-ttu-id="1e4a6-197">För **aktivitetskategorin**väljer **konto Användaretablering**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-197">For **Activity Category**, select **Account User Provisioning**.</span></span>         |
| <span data-ttu-id="1e4a6-198">Status för förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="1e4a6-198">Password rollover status</span></span>             | <span data-ttu-id="1e4a6-199">För **aktivitetskategorin**väljer **automatisk förnyelse av App lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-199">For **Activity Category**, select **Automatic App Password Rollover**.</span></span>      |
| <span data-ttu-id="1e4a6-200">Kontoetableringsfel</span><span class="sxs-lookup"><span data-stu-id="1e4a6-200">Account provisioning errors</span></span>          | <span data-ttu-id="1e4a6-201">För **aktivitetskategorin**väljer **konto Användaretablering**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-201">For **Activity Category**, select **Account User Provisioning**.</span></span>        |
| <span data-ttu-id="1e4a6-202">Ändringar av Office365 namn</span><span class="sxs-lookup"><span data-stu-id="1e4a6-202">Office365 Group Name Changes</span></span>         | <span data-ttu-id="1e4a6-203">För **aktivitetskategorin**väljer **Self-service lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-203">For **Activity Category**, select **Self-service Password Management**.</span></span> <span data-ttu-id="1e4a6-204">För **aktivitet resurstypen**väljer **gruppen**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-204">For **Activity Resource Type**, select **Group**.</span></span> <span data-ttu-id="1e4a6-205">För **Aktivitetskälla**väljer **O365 grupper**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-205">For **Activity Source**, select **O365 groups**.</span></span>|

<span data-ttu-id="1e4a6-206">tooview hello **programanvändning** rapport om hello **Azure Active Directory** bladet under **hantera**väljer **företagsprogram**, och välj sedan **inloggningar**.</span><span class="sxs-lookup"><span data-stu-id="1e4a6-206">tooview hello **Application Usage** report, on hello **Azure Active Directory** blade, under **MANAGE**, select **Enterprise Applications**, and then select **Sign-ins**.</span></span>


<span data-ttu-id="1e4a6-207">![Enterprise program inloggningar rapporten](./media/active-directory-reporting-migration/199.png "Enterprise program inloggningar rapport")</span><span class="sxs-lookup"><span data-stu-id="1e4a6-207">![Enterprise Applications Sign-Ins report](./media/active-directory-reporting-migration/199.png "Enterprise Applications Sign-Ins report")</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e4a6-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e4a6-208">Next steps</span></span>

<span data-ttu-id="1e4a6-209">En översikt över rapportering finns i hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1e4a6-209">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
