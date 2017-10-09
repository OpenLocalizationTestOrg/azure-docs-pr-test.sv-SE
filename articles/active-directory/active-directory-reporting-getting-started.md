---
title: "Azure Active Directory Reporting: Komma igång | Microsoft Docs"
description: "Visar hello olika tillgängliga rapporterna i Azure Active Directory reporting"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: f47875708398391dd7f3efdc56a741fdba273b76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a><span data-ttu-id="c149e-103">Komma igång med Azure Active Directory Reporting</span><span class="sxs-lookup"><span data-stu-id="c149e-103">Getting started with Azure Active Directory Reporting</span></span>
## <a name="what-it-is"></a><span data-ttu-id="c149e-104">Vad det är</span><span class="sxs-lookup"><span data-stu-id="c149e-104">What it is</span></span>
<span data-ttu-id="c149e-105">Azure AD (Active Directory Azure) tillhandahåller säkerhets-, aktivitets- och granskningsrapporter för din katalog.</span><span class="sxs-lookup"><span data-stu-id="c149e-105">Azure Active Directory (Azure AD) includes security, activity, and audit reports for your directory.</span></span> <span data-ttu-id="c149e-106">Här är en lista över rapporter hello:</span><span class="sxs-lookup"><span data-stu-id="c149e-106">Here's a list of hello reports included:</span></span>

### <a name="security-reports"></a><span data-ttu-id="c149e-107">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="c149e-107">Security reports</span></span>
* <span data-ttu-id="c149e-108">Inloggningar från okända källor</span><span class="sxs-lookup"><span data-stu-id="c149e-108">Sign-ins from unknown sources</span></span>
* <span data-ttu-id="c149e-109">Inloggningar efter flera fel</span><span class="sxs-lookup"><span data-stu-id="c149e-109">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="c149e-110">Inloggningar från flera geografiska områden</span><span class="sxs-lookup"><span data-stu-id="c149e-110">Sign-ins from multiple geographies</span></span>
* <span data-ttu-id="c149e-111">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="c149e-111">Sign-ins from IP addresses with suspicious activity</span></span>
* <span data-ttu-id="c149e-112">Oregelbunden inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="c149e-112">Irregular sign-in activity</span></span>
* <span data-ttu-id="c149e-113">Inloggningar från potentiellt infekterade enheter</span><span class="sxs-lookup"><span data-stu-id="c149e-113">Sign-ins from possibly infected devices</span></span>
* <span data-ttu-id="c149e-114">Användare med avvikande inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="c149e-114">Users with anomalous sign-in activity</span></span>

### <a name="activity-reports"></a><span data-ttu-id="c149e-115">Aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="c149e-115">Activity reports</span></span>
* <span data-ttu-id="c149e-116">Programanvändning: sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c149e-116">Application usage: summary</span></span>
* <span data-ttu-id="c149e-117">Programanvändning: detaljerad</span><span class="sxs-lookup"><span data-stu-id="c149e-117">Application usage: detailed</span></span>
* <span data-ttu-id="c149e-118">Instrumentpanel för program</span><span class="sxs-lookup"><span data-stu-id="c149e-118">Application dashboard</span></span>
* <span data-ttu-id="c149e-119">Kontoetableringsfel</span><span class="sxs-lookup"><span data-stu-id="c149e-119">Account provisioning errors</span></span>
* <span data-ttu-id="c149e-120">Enskilda användarenheter</span><span class="sxs-lookup"><span data-stu-id="c149e-120">Individual user devices</span></span>
* <span data-ttu-id="c149e-121">Aktivitet för enskilda användare</span><span class="sxs-lookup"><span data-stu-id="c149e-121">Individual user Activity</span></span>
* <span data-ttu-id="c149e-122">Aktivitetsrapport för grupper</span><span class="sxs-lookup"><span data-stu-id="c149e-122">Groups activity report</span></span>
* <span data-ttu-id="c149e-123">Aktivitetsrapport över registrering av lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="c149e-123">Password Reset Registration Activity Report</span></span>
* <span data-ttu-id="c149e-124">Lösenordsåterställningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="c149e-124">Password reset activity</span></span>

### <a name="audit-reports"></a><span data-ttu-id="c149e-125">Granskningsrapporter</span><span class="sxs-lookup"><span data-stu-id="c149e-125">Audit reports</span></span>
* <span data-ttu-id="c149e-126">Kataloggranskningsrapport</span><span class="sxs-lookup"><span data-stu-id="c149e-126">Directory audit report</span></span>

> [!TIP]
> <span data-ttu-id="c149e-127">Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-127">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="how-it-works"></a><span data-ttu-id="c149e-128">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="c149e-128">How it works</span></span>
### <a name="reporting-pipeline"></a><span data-ttu-id="c149e-129">Rapporteringsflöde</span><span class="sxs-lookup"><span data-stu-id="c149e-129">Reporting pipeline</span></span>
<span data-ttu-id="c149e-130">Hej rapporteringsflödet består av tre steg.</span><span class="sxs-lookup"><span data-stu-id="c149e-130">hello reporting pipeline consists of three main steps.</span></span> <span data-ttu-id="c149e-131">Varje gång en användare loggar in, eller en autentisering görs, händer följande hello:</span><span class="sxs-lookup"><span data-stu-id="c149e-131">Every time a user signs in, or an authentication is made, hello following happens:</span></span>

* <span data-ttu-id="c149e-132">Först hello användaren har autentiserats (lyckas eller misslyckas) och hello resultatet lagras i hello Azure Active Directory service-databaser.</span><span class="sxs-lookup"><span data-stu-id="c149e-132">First, hello user is authenticated (successfully or unsuccessfully), and hello result is stored in hello Azure Active Directory service databases.</span></span>
* <span data-ttu-id="c149e-133">De senaste inloggningarna bearbetas med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="c149e-133">At regular intervals, all recent sign ins are processed.</span></span> <span data-ttu-id="c149e-134">I detta läge söker våra säkerhetsalgoritmer och algoritmer för avvikande aktivitet igenom de senaste inloggningarna och letar efter misstänkt aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c149e-134">At this point, our security and anomalous activity algorithms are searching all recent sign ins for suspicious activity.</span></span>
* <span data-ttu-id="c149e-135">Efter bearbetningen hello rapporter skrivs, cachelagras och hanteras i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c149e-135">After processing, hello reports are written, cached, and served in hello Azure classic portal.</span></span>

### <a name="report-generation-times"></a><span data-ttu-id="c149e-136">Rapportera genereringstider</span><span class="sxs-lookup"><span data-stu-id="c149e-136">Report generation times</span></span>
<span data-ttu-id="c149e-137">På grund av toohello stora mängden autentiseringar och logga moduler som bearbetas av hello Azure AD-plattformen, är hello senaste inloggningar som bearbetas i genomsnitt en timme gamla.</span><span class="sxs-lookup"><span data-stu-id="c149e-137">Due toohello large volume of authentications and sign ins processed by hello Azure AD platform, hello most recent sign-ins processed are, on average, one hour old.</span></span> <span data-ttu-id="c149e-138">I sällsynta fall kan det ta upp too8 timmar tooprocess hello senaste inloggningar.</span><span class="sxs-lookup"><span data-stu-id="c149e-138">In rare cases, it may take up too8 hours tooprocess hello most recent sign-ins.</span></span>

<span data-ttu-id="c149e-139">Du kan hitta hello senast bearbetade inloggningen genom att undersöka hello hjälptexten hello överst i varje rapport.</span><span class="sxs-lookup"><span data-stu-id="c149e-139">You can find hello most recent processed sign-in by examining hello help text at hello top of each report.</span></span>

![Hjälptexten hello överst i varje rapport](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> <span data-ttu-id="c149e-141">Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-141">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="getting-started"></a><span data-ttu-id="c149e-142">Komma igång</span><span class="sxs-lookup"><span data-stu-id="c149e-142">Getting started</span></span>
### <a name="sign-into-hello-azure-classic-portal"></a><span data-ttu-id="c149e-143">Logga in på hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c149e-143">Sign into hello Azure classic portal</span></span>
<span data-ttu-id="c149e-144">Först behöver du toosign till hello [klassiska Azure-portalen](https://manage.windowsazure.com) som en global administratör eller efterlevnadsadministratör.</span><span class="sxs-lookup"><span data-stu-id="c149e-144">First, you'll need toosign into hello [Azure classic portal](https://manage.windowsazure.com)  as a global or compliance administrator.</span></span> <span data-ttu-id="c149e-145">Du måste också vara ett Azure-prenumeration tjänstadministratör eller en medadministratör eller använda hello ”åtkomst tooAzure AD” Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c149e-145">You must also be an Azure subscription service administrator or co-administrator, or be using hello "Access tooAzure AD" Azure subscription.</span></span>

### <a name="navigate-tooreports"></a><span data-ttu-id="c149e-146">Navigera tooReports</span><span class="sxs-lookup"><span data-stu-id="c149e-146">Navigate tooReports</span></span>
<span data-ttu-id="c149e-147">tooview rapporter, navigera toohello rapporter fliken hello överst i din katalog.</span><span class="sxs-lookup"><span data-stu-id="c149e-147">tooview Reports, navigate toohello Reports tab at hello top of your directory.</span></span>

<span data-ttu-id="c149e-148">Om det här är första gången du visar hello rapporter behöver tooagree tooa dialogrutan innan du kan visa hello rapporter.</span><span class="sxs-lookup"><span data-stu-id="c149e-148">If this is your first time viewing hello reports, you'll need tooagree tooa dialog box before you can view hello reports.</span></span> <span data-ttu-id="c149e-149">Detta är att den är godkänd för administratörer i din organisation tooview tooensure dessa data, som kan betraktas som privat information i vissa länder.</span><span class="sxs-lookup"><span data-stu-id="c149e-149">This is tooensure that it's acceptable for admins in your organization tooview this data, which may be considered private information in some countries.</span></span>

![Dialogruta](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a><span data-ttu-id="c149e-151">Utforska varje rapport</span><span class="sxs-lookup"><span data-stu-id="c149e-151">Explore each report</span></span>
<span data-ttu-id="c149e-152">Navigera till varje rapport toosee hello data som samlas in och hello inloggningar som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="c149e-152">Navigate into each report toosee hello data being collected and hello sign-ins processed.</span></span> <span data-ttu-id="c149e-153">Du hittar en [lista över alla hello rapporter här](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-153">You can find a [list of all hello reports here](active-directory-reporting-guide.md).</span></span>

![Alla rapporter](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-hello-reports-as-csv"></a><span data-ttu-id="c149e-155">Hämta hello rapporter som CSV-fil</span><span class="sxs-lookup"><span data-stu-id="c149e-155">Download hello reports as CSV</span></span>
<span data-ttu-id="c149e-156">Alla rapporter kan laddas ned som CSV-filer (kommaavgränsade).</span><span class="sxs-lookup"><span data-stu-id="c149e-156">Each report can be downloaded as a CSV (comma-separated value) file.</span></span> <span data-ttu-id="c149e-157">Du kan använda de här filerna i Excel, PowerBI eller tredje parts analys program toofurther analysera dina data.</span><span class="sxs-lookup"><span data-stu-id="c149e-157">You can use these files in Excel, PowerBI or third-party analysis programs toofurther analyze your data.</span></span>

<span data-ttu-id="c149e-158">toodownload någon rapport som en CSV-fil, navigera toohello rapporten och klicka på ”ladda ned” längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c149e-158">toodownload any report as a CSV, navigate toohello report and click "Download" at hello bottom.</span></span>

![Knappen Ladda ned](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> <span data-ttu-id="c149e-160">Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-160">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="c149e-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c149e-161">Next steps</span></span>
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a><span data-ttu-id="c149e-162">Anpassa aviseringar om avvikande inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="c149e-162">Customize alerts for anomalous sign in activity</span></span>
<span data-ttu-id="c149e-163">Navigera toohello ”konfigurera” fliken i din katalog.</span><span class="sxs-lookup"><span data-stu-id="c149e-163">Navigate toohello "Configure" tab of your directory.</span></span>

<span data-ttu-id="c149e-164">Rulla toohello ”meddelanden” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c149e-164">Scroll toohello "Notifications" section.</span></span>

<span data-ttu-id="c149e-165">Aktivera eller inaktivera hello ”e-postaviseringar av avvikande inloggningar” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c149e-165">Enable or disable hello "Email Notifications of Anomalous sign-ins" section.</span></span>

![hello meddelanden](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-hello-azure-ad-reporting-api"></a><span data-ttu-id="c149e-167">Integrera med hello Azure AD Reporting API</span><span class="sxs-lookup"><span data-stu-id="c149e-167">Integrate with hello Azure AD Reporting API</span></span>
<span data-ttu-id="c149e-168">Se [komma igång med hello Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-168">See [Getting started with hello Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

### <a name="engage-multi-factor-authentication-on-users"></a><span data-ttu-id="c149e-169">Aktivera Multi-Factor Authentication för användare</span><span class="sxs-lookup"><span data-stu-id="c149e-169">Engage Multi-Factor Authentication on users</span></span>
<span data-ttu-id="c149e-170">Välj en användare i en rapport.</span><span class="sxs-lookup"><span data-stu-id="c149e-170">Select a user in a report.</span></span>

<span data-ttu-id="c149e-171">Klicka hello ”aktivera MFA” längst ned hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="c149e-171">Click hello "Enable MFA" button at hello bottom of hello screen.</span></span>

![Hej Multi-Factor Authentication-knappen längst ned hello hello-skärmen](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> <span data-ttu-id="c149e-173">Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-173">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="learn-more"></a><span data-ttu-id="c149e-174">Läs mer</span><span class="sxs-lookup"><span data-stu-id="c149e-174">Learn more</span></span>
### <a name="audit-events"></a><span data-ttu-id="c149e-175">Granska händelser</span><span class="sxs-lookup"><span data-stu-id="c149e-175">Audit events</span></span>
<span data-ttu-id="c149e-176">Lär dig mer om vilka händelser som granskas i hello directory i [Azure Active Directory Reporting granskningshändelser](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-176">Learn about what events are audited in hello directory in [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md).</span></span>

### <a name="api-integration"></a><span data-ttu-id="c149e-177">API-integrering</span><span class="sxs-lookup"><span data-stu-id="c149e-177">API Integration</span></span>
<span data-ttu-id="c149e-178">Se [komma igång med hello Reporting API](active-directory-reporting-api-getting-started.md) och hello [API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span><span class="sxs-lookup"><span data-stu-id="c149e-178">See [Getting started with hello Reporting API](active-directory-reporting-api-getting-started.md) and hello [API reference documentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span></span>

### <a name="get-in-touch"></a><span data-ttu-id="c149e-179">Kontakta oss</span><span class="sxs-lookup"><span data-stu-id="c149e-179">Get in touch</span></span>
<span data-ttu-id="c149e-180">E-post [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) för feedback, hjälp eller eventuella frågor.</span><span class="sxs-lookup"><span data-stu-id="c149e-180">Email [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) for feedback, help, or any questions you might have.</span></span>

> [!TIP]
> <span data-ttu-id="c149e-181">Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="c149e-181">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

