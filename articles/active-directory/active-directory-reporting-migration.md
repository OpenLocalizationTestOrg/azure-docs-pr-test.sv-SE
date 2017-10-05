---
title: Hitta aktivitetsrapporter i Azure portal | Microsoft Docs
description: "Lär dig att hitta Azure Active Directory aktivitetsrapporter i Azure-portalen."
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
ms.openlocfilehash: f1875582476c3817b9eb0082b6548cc15043cb98
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="find-activity-reports-in-the-azure-portal"></a><span data-ttu-id="1e1a7-103">Hitta aktivitetsrapporter i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e1a7-103">Find activity reports in the Azure portal</span></span>

<span data-ttu-id="1e1a7-104">Om du flyttar från den klassiska Azure-portalen till Azure portal, får du en ny titt på Azure Active Directory (AD Azure) aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-104">If you are moving from the Azure classic portal to the Azure portal, you get a new look at Azure Active Directory (Azure AD) activity logs.</span></span> <span data-ttu-id="1e1a7-105">I en senaste [blogginlägget](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), förklarar vi hur du kan se aktiviteten loggar i kontexten för den resurs som du arbetar med i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-105">In a recent [blog post](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), we explain how you can see activity logs in the context of the resource you are working on in the Azure portal.</span></span> <span data-ttu-id="1e1a7-106">I den här artikeln beskrivs hur du hittar rapporter som du använde i den klassiska Azure-portalen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-106">In this article, we describe how to find reports that you used in the Azure classic portal in the Azure portal.</span></span>

## <a name="whats-new"></a><span data-ttu-id="1e1a7-107">Nyheter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-107">What's new</span></span>

<span data-ttu-id="1e1a7-108">Rapporter i den klassiska Azure-portalen är uppdelade i kategorier:</span><span class="sxs-lookup"><span data-stu-id="1e1a7-108">Reports in the Azure classic portal are separated into categories:</span></span>

1.  <span data-ttu-id="1e1a7-109">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-109">Security reports</span></span>
2.  <span data-ttu-id="1e1a7-110">Aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-110">Activity reports</span></span>
3.  <span data-ttu-id="1e1a7-111">Integrerad apprapporter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-111">Integrated app reports</span></span>

### <a name="activity-and-integrated-app-reports"></a><span data-ttu-id="1e1a7-112">Aktiviteten och integrerad apprapporter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-112">Activity and integrated app reports</span></span>

<span data-ttu-id="1e1a7-113">För sammanhangsberoende rapportering i Azure-portalen, kombineras befintliga rapporter till en enda vy.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-113">For context-based reporting in the Azure portal, existing reports are merged into a single view.</span></span> <span data-ttu-id="1e1a7-114">En enda underliggande API ger data till vyn.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-114">A single, underlying API provides the data to the view.</span></span>

<span data-ttu-id="1e1a7-115">Visa den här vyn på den **Azure Active Directory** bladet under **AKTIVITETEN**väljer **granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-115">To see this view, on the **Azure Active Directory** blade, under **ACTIVITY**, select **Audit logs**.</span></span>

<span data-ttu-id="1e1a7-116">![Granskningsloggar](./media/active-directory-reporting-migration/482.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-116">![Audit logs](./media/active-directory-reporting-migration/482.png "Audit logs")</span></span>

<span data-ttu-id="1e1a7-117">Följande rapporter konsolideras i den här vyn:</span><span class="sxs-lookup"><span data-stu-id="1e1a7-117">The following reports are consolidated in this view:</span></span>

-   <span data-ttu-id="1e1a7-118">Granska rapporten</span><span class="sxs-lookup"><span data-stu-id="1e1a7-118">Audit report</span></span>
-   <span data-ttu-id="1e1a7-119">Lösenordsåterställningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-119">Password reset activity</span></span>
-   <span data-ttu-id="1e1a7-120">Lösenordsåterställningsaktivitet för registrering</span><span class="sxs-lookup"><span data-stu-id="1e1a7-120">Password reset registration activity</span></span>
-   <span data-ttu-id="1e1a7-121">Självbetjäning grupper aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-121">Self-service groups activity</span></span>
-   <span data-ttu-id="1e1a7-122">Ändringar av Office365 namn</span><span class="sxs-lookup"><span data-stu-id="1e1a7-122">Office365 Group Name Changes</span></span>
-   <span data-ttu-id="1e1a7-123">Kontot etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-123">Account provisioning activity</span></span>
-   <span data-ttu-id="1e1a7-124">Status för förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="1e1a7-124">Password rollover status</span></span>
-   <span data-ttu-id="1e1a7-125">Kontoetableringsfel</span><span class="sxs-lookup"><span data-stu-id="1e1a7-125">Account provisioning errors</span></span>


<span data-ttu-id="1e1a7-126">Programanvändning rapporten har förbättrats och ingår i den **inloggningar** vyn.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-126">The Application Usage report has been enhanced and is included in the **Sign-ins** view.</span></span> <span data-ttu-id="1e1a7-127">Visa den här vyn på den **Azure Active Directory** bladet under **AKTIVITETEN**väljer **inloggningar**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-127">To see this view, on the **Azure Active Directory** blade, under **ACTIVITY**, select **Sign-ins**.</span></span>

<span data-ttu-id="1e1a7-128">![Inloggningar visa](./media/active-directory-reporting-migration/483.png "inloggningar visa")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-128">![Sign-ins view](./media/active-directory-reporting-migration/483.png "Sign-ins view")</span></span>

<span data-ttu-id="1e1a7-129">Den **inloggningar** vyn innehåller alla användarinloggningar.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-129">The **Sign-ins** view includes all user sign-ins.</span></span> <span data-ttu-id="1e1a7-130">Du kan använda den här informationen för att hämta information om användning.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-130">You can use this information to get application usage information.</span></span> <span data-ttu-id="1e1a7-131">Du kan också visa information om användning av program i den **företagsprogram** översikt över i den **hantera** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-131">You also can view application usage information in the **Enterprise applications** overview, in the **MANAGE** section.</span></span>

<span data-ttu-id="1e1a7-132">![Företagsprogram](./media/active-directory-reporting-migration/484.png "företagsprogram")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-132">![Enterprise applications](./media/active-directory-reporting-migration/484.png "Enterprise applications")</span></span>

## <a name="access-a-specific-report"></a><span data-ttu-id="1e1a7-133">Åtkomst till en rapport</span><span class="sxs-lookup"><span data-stu-id="1e1a7-133">Access a specific report</span></span>

<span data-ttu-id="1e1a7-134">Även om Azure-portalen erbjuder en enda vy kan titta du också på specifika rapporter.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-134">Although the Azure portal offers a single view, you also can look at specific reports.</span></span>

### <a name="audit-logs"></a><span data-ttu-id="1e1a7-135">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="1e1a7-135">Audit logs</span></span>

<span data-ttu-id="1e1a7-136">Som svar på kundfeedback i Azure-portalen, kan du använda avancerade filter att komma åt data som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-136">In response to customer feedback, in the Azure portal, you can use advanced filtering to access the data you want.</span></span> <span data-ttu-id="1e1a7-137">Är ett filter som du kan använda en *aktivitetskategorin*, som listar de olika typerna av aktiviteten loggar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-137">One filter you can use is an *activity category*, which lists the different types of activity logs in Azure AD.</span></span> <span data-ttu-id="1e1a7-138">Du kan välja en kategori för att begränsa resultaten till vad du letar efter.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-138">To narrow results to what you are looking for, you can select a category.</span></span>

<span data-ttu-id="1e1a7-139">Till exempel om du är intresserad av att endast aktiviteter som rör lösenordsåterställning med självbetjäning kan du välja den **Self-service lösenordshantering** kategori.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-139">For example, if you are interested only in activities related to self-service password resets, you can choose the **Self-service Password Management** category.</span></span> <span data-ttu-id="1e1a7-140">De kategorier som du ser baseras på den resurs som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-140">The categories you see are based on the resource you are working in.</span></span>  

<span data-ttu-id="1e1a7-141">![Kategori alternativen på sidan Filter granskningsloggar](./media/active-directory-reporting-migration/06.png "kategori alternativen på sidan Filter granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-141">![Category options on the Filter Audit Logs page](./media/active-directory-reporting-migration/06.png "Category options on the Filter Audit Logs page")</span></span>

<span data-ttu-id="1e1a7-142">Aktivitetskategorier inkluderar:</span><span class="sxs-lookup"><span data-stu-id="1e1a7-142">Activity categories include:</span></span>

- <span data-ttu-id="1e1a7-143">Kärnkatalog</span><span class="sxs-lookup"><span data-stu-id="1e1a7-143">Core Directory</span></span>
- <span data-ttu-id="1e1a7-144">Hantering av lösenord för självbetjäning</span><span class="sxs-lookup"><span data-stu-id="1e1a7-144">Self-service Password Management</span></span>
- <span data-ttu-id="1e1a7-145">Självbetjäning, grupphantering</span><span class="sxs-lookup"><span data-stu-id="1e1a7-145">Self-service Group Management</span></span>
- <span data-ttu-id="1e1a7-146">Kontoetablering</span><span class="sxs-lookup"><span data-stu-id="1e1a7-146">Account Provisioning</span></span>

### <a name="application-usage"></a><span data-ttu-id="1e1a7-147">Programanvändning</span><span class="sxs-lookup"><span data-stu-id="1e1a7-147">Application usage</span></span>

<span data-ttu-id="1e1a7-148">Visa information om programanvändning för alla appar eller för en enda app under **AKTIVITETEN**väljer **inloggningar**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-148">To view details about application usage for all apps or for a single app, under **ACTIVITY**, select **Sign-ins**.</span></span> <span data-ttu-id="1e1a7-149">Om du vill begränsa sökresultaten kan du filtrera efter användarnamn eller programnamn.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-149">To narrow the results, you can filter on user name or application name.</span></span>

<span data-ttu-id="1e1a7-150">![Filter inloggning händelser sidan](./media/active-directory-reporting-migration/07.png "filtrera inloggning händelser sida")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-150">![Filter Sign-In Events page](./media/active-directory-reporting-migration/07.png "Filter Sign-In Events page")</span></span>

### <a name="security-reports"></a><span data-ttu-id="1e1a7-151">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-151">Security reports</span></span>

#### <a name="azure-ad-anomalous-activity-reports"></a><span data-ttu-id="1e1a7-152">Azure AD avvikande aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-152">Azure AD anomalous activity reports</span></span>

<span data-ttu-id="1e1a7-153">Säkerheten i Azure AD avvikande aktivitet har konsoliderade rapporter från den klassiska Azure-portalen för att ge dig en, centrala vy.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-153">Azure AD anomalous activity security reports from the Azure classic portal have been consolidated to provide you with one, central view.</span></span> <span data-ttu-id="1e1a7-154">Den här vyn visar alla riskhändelser som säkerhetsrelaterade att Azure AD kan identifiera och rapportera om.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-154">This view shows all security-related risk events that Azure AD can detect and report on.</span></span>

<span data-ttu-id="1e1a7-155">Följande tabell listar Azure AD avvikande aktivitet säkerhetsrapporter och motsvarande risk händelsetyper i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-155">The following table lists the Azure AD anomalous activity security reports, and corresponding risk event types in the Azure portal.</span></span>

| <span data-ttu-id="1e1a7-156">Azure AD avvikande Aktivitetsrapport</span><span class="sxs-lookup"><span data-stu-id="1e1a7-156">Azure AD anomalous activity report</span></span> |  <span data-ttu-id="1e1a7-157">Identity protection risk händelsetyp</span><span class="sxs-lookup"><span data-stu-id="1e1a7-157">Identity protection risk event type</span></span>|
| :--- | :--- |
| <span data-ttu-id="1e1a7-158">Används med läckta autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-158">Users with leaked credentials</span></span> | <span data-ttu-id="1e1a7-159">Läckta autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-159">Leaked credentials</span></span> |
| <span data-ttu-id="1e1a7-160">Oregelbunden inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-160">Irregular sign-in activity</span></span> | <span data-ttu-id="1e1a7-161">Omöjligt att resa till ovanliga platser</span><span class="sxs-lookup"><span data-stu-id="1e1a7-161">Impossible travel to atypical locations</span></span> |
| <span data-ttu-id="1e1a7-162">Inloggningar från potentiellt infekterade enheter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-162">Sign-ins from possibly infected devices</span></span> | <span data-ttu-id="1e1a7-163">Inloggningar från angripna enheter</span><span class="sxs-lookup"><span data-stu-id="1e1a7-163">Sign-ins from infected devices</span></span>|
| <span data-ttu-id="1e1a7-164">Inloggningar från okända källor</span><span class="sxs-lookup"><span data-stu-id="1e1a7-164">Sign-ins from unknown sources</span></span> | <span data-ttu-id="1e1a7-165">Inloggningar från anonyma IP-adresser</span><span class="sxs-lookup"><span data-stu-id="1e1a7-165">Sign-ins from anonymous IP addresses</span></span> |
| <span data-ttu-id="1e1a7-166">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-166">Sign-ins from IP addresses with suspicious activity</span></span> | <span data-ttu-id="1e1a7-167">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-167">Sign-ins from IP addresses with suspicious activity</span></span> |
| - | <span data-ttu-id="1e1a7-168">Inloggningar från okända platser</span><span class="sxs-lookup"><span data-stu-id="1e1a7-168">Sign-ins from unfamiliar locations</span></span> |

<span data-ttu-id="1e1a7-169">Följande Azure AD avvikande aktivitet säkerheten rapporterar inte ingår som riskhändelser i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="1e1a7-169">The following Azure AD anomalous activity security reports are not included as risk events in the Azure portal:</span></span>

* <span data-ttu-id="1e1a7-170">Inloggningar efter flera fel</span><span class="sxs-lookup"><span data-stu-id="1e1a7-170">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="1e1a7-171">Inloggningar från flera geografiska områden</span><span class="sxs-lookup"><span data-stu-id="1e1a7-171">Sign-ins from multiple geographies</span></span>

<span data-ttu-id="1e1a7-172">Dessa rapporter är fortfarande tillgängliga i den klassiska Azure-portalen, men de kommer att inaktualiseras i framtiden.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-172">These reports are still available in the Azure classic portal, but they will be deprecated at some time in the future.</span></span>

<span data-ttu-id="1e1a7-173">Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="1e1a7-173">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>  


#### <a name="detected-risk-events"></a><span data-ttu-id="1e1a7-174">Identifierade riskhändelser</span><span class="sxs-lookup"><span data-stu-id="1e1a7-174">Detected risk events</span></span>

<span data-ttu-id="1e1a7-175">I Azure-portalen kan du komma åt rapporter om identifierad riskhändelser på den **Azure Active Directory** bladet under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-175">In the Azure portal, you can access reports about detected risk events on the **Azure Active Directory** blade, under **SECURITY**.</span></span> <span data-ttu-id="1e1a7-176">Identifierade riskhändelser spåras i följande rapporter:</span><span class="sxs-lookup"><span data-stu-id="1e1a7-176">Detected risk events are tracked in the following reports:</span></span>   

- <span data-ttu-id="1e1a7-177">Användare i riskzonen</span><span class="sxs-lookup"><span data-stu-id="1e1a7-177">Users at Risk</span></span>
- <span data-ttu-id="1e1a7-178">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="1e1a7-178">Risky Sign-ins</span></span>

<span data-ttu-id="1e1a7-179">![Säkerhetsrapporter](./media/active-directory-reporting-migration/04.png "säkerhetsrapporter")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-179">![Security reports](./media/active-directory-reporting-migration/04.png "Security reports")</span></span>

<span data-ttu-id="1e1a7-180">Mer information om säkerhetsrapporter finns:</span><span class="sxs-lookup"><span data-stu-id="1e1a7-180">For more information about security reports, see:</span></span>

- [<span data-ttu-id="1e1a7-181">Användare på Risk säkerhetsrapporten i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="1e1a7-181">Users at Risk security report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="1e1a7-182">Riskfyllda inloggningar rapporten i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="1e1a7-182">Risky Sign-ins report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-the-azure-classic-portal-vs-the-azure-portal"></a><span data-ttu-id="1e1a7-183">Aktivitetsrapporter i den klassiska Azure-portalen och Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e1a7-183">Activity reports in the Azure classic portal vs. the Azure portal</span></span>

<span data-ttu-id="1e1a7-184">Tabellen i det här avsnittet innehåller befintliga rapporter i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-184">The table in this section lists existing reports in the Azure classic portal.</span></span> <span data-ttu-id="1e1a7-185">Här beskrivs också hur du kan få samma information i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-185">It also describes how you can get the same information in the Azure portal.</span></span>

<span data-ttu-id="1e1a7-186">Visa alla granskning data på den **Azure Active Directory** bladet under **AKTIVITETEN**, gå till **granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-186">To view all auditing data, on the **Azure Active Directory** blade, under **ACTIVITY**, go to **Audit logs**.</span></span>

<span data-ttu-id="1e1a7-187">![Granskningsloggar](./media/active-directory-reporting-migration/61.png "Granskningsloggar")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-187">![Audit logs](./media/active-directory-reporting-migration/61.png "Audit logs")</span></span>

| <span data-ttu-id="1e1a7-188">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="1e1a7-188">Azure classic portal</span></span>                 | <span data-ttu-id="1e1a7-189">Du hittar i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e1a7-189">To find in the Azure portal</span></span>                                                         |
| ---                                  | ---                                                                        |
| <span data-ttu-id="1e1a7-190">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="1e1a7-190">Audit logs</span></span>                           | <span data-ttu-id="1e1a7-191">För **aktivitetskategorin**väljer **Core Directory**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-191">For **Activity Category**, select **Core Directory**.</span></span>                       |
| <span data-ttu-id="1e1a7-192">Lösenordsåterställningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-192">Password reset activity</span></span>              | <span data-ttu-id="1e1a7-193">För **aktivitetskategorin**väljer **Self-service lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-193">For **Activity Category**, select **Self-service Password Management**.</span></span> |
| <span data-ttu-id="1e1a7-194">Lösenordsåterställningsaktivitet för registrering</span><span class="sxs-lookup"><span data-stu-id="1e1a7-194">Password reset registration activity</span></span> | <span data-ttu-id="1e1a7-195">För **aktivitetskategorin**väljer **Self-service lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-195">For **Activity Category**, select **Self-service Password Management**.</span></span>     |
| <span data-ttu-id="1e1a7-196">Självbetjäning grupper aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-196">Self-service groups activity</span></span>         | <span data-ttu-id="1e1a7-197">För **aktivitetskategorin**väljer **Self-service Group Management**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-197">For **Activity Category**, select **Self-service Group Management**.</span></span>        |
| <span data-ttu-id="1e1a7-198">Kontot etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="1e1a7-198">Account provisioning activity</span></span>        | <span data-ttu-id="1e1a7-199">För **aktivitetskategorin**väljer **konto Användaretablering**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-199">For **Activity Category**, select **Account User Provisioning**.</span></span>         |
| <span data-ttu-id="1e1a7-200">Status för förnyelse av lösenord</span><span class="sxs-lookup"><span data-stu-id="1e1a7-200">Password rollover status</span></span>             | <span data-ttu-id="1e1a7-201">För **aktivitetskategorin**väljer **automatisk förnyelse av App lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-201">For **Activity Category**, select **Automatic App Password Rollover**.</span></span>      |
| <span data-ttu-id="1e1a7-202">Kontoetableringsfel</span><span class="sxs-lookup"><span data-stu-id="1e1a7-202">Account provisioning errors</span></span>          | <span data-ttu-id="1e1a7-203">För **aktivitetskategorin**väljer **konto Användaretablering**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-203">For **Activity Category**, select **Account User Provisioning**.</span></span>        |
| <span data-ttu-id="1e1a7-204">Ändringar av Office365 namn</span><span class="sxs-lookup"><span data-stu-id="1e1a7-204">Office365 Group Name Changes</span></span>         | <span data-ttu-id="1e1a7-205">För **aktivitetskategorin**väljer **Self-service lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-205">For **Activity Category**, select **Self-service Password Management**.</span></span> <span data-ttu-id="1e1a7-206">För **aktivitet resurstypen**väljer **gruppen**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-206">For **Activity Resource Type**, select **Group**.</span></span> <span data-ttu-id="1e1a7-207">För **Aktivitetskälla**väljer **O365 grupper**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-207">For **Activity Source**, select **O365 groups**.</span></span>|

<span data-ttu-id="1e1a7-208">Visa den **programanvändning** rapportera på den **Azure Active Directory** bladet under **hantera**väljer **företagsprogram**, och välj sedan **inloggningar**.</span><span class="sxs-lookup"><span data-stu-id="1e1a7-208">To view the **Application Usage** report, on the **Azure Active Directory** blade, under **MANAGE**, select **Enterprise Applications**, and then select **Sign-ins**.</span></span>


<span data-ttu-id="1e1a7-209">![Enterprise program inloggningar rapporten](./media/active-directory-reporting-migration/199.png "Enterprise program inloggningar rapport")</span><span class="sxs-lookup"><span data-stu-id="1e1a7-209">![Enterprise Applications Sign-Ins report](./media/active-directory-reporting-migration/199.png "Enterprise Applications Sign-Ins report")</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e1a7-210">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e1a7-210">Next steps</span></span>

<span data-ttu-id="1e1a7-211">En översikt över rapportering finns i [Azure Active Directory-rapportering](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1e1a7-211">For an overview of reporting, see the [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
