---
title: "Optimera din Active Directory-miljö med Azure Log Analytics | Microsoft Docs"
description: "Du kan använda Active Directory-bedömning-lösning för att bedöma risken och hälsotillståndet för server-miljöer med regelbundna intervall."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97368f0b9e89ffd0cd982b6e8670d5a1f62ad42c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a><span data-ttu-id="1a3c5-103">Optimera din Active Directory-miljö med Active Directory-bedömning lösningen i logganalys</span><span class="sxs-lookup"><span data-stu-id="1a3c5-103">Optimize your Active Directory environment with the Active Directory Assessment solution in Log Analytics</span></span>

![AD-bedömning symbol](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

<span data-ttu-id="1a3c5-105">Du kan använda Active Directory-bedömning-lösning för att bedöma risken och hälsotillståndet för server-miljöer med regelbundna intervall.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-105">You can use the Active Directory Assessment solution to assess the risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="1a3c5-106">Den här artikeln hjälper dig att installera och använda lösningen så att du kan vidta åtgärder för möjliga problem.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-106">This article helps you install and use the solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="1a3c5-107">Den här lösningen ger en prioriterad lista med rekommendationer som är specifika för din distribuerade serverinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-107">This solution provides a prioritized list of recommendations specific to your deployed server infrastructure.</span></span> <span data-ttu-id="1a3c5-108">Rekommendationerna kategoriseras i fyra fokusområden, som hjälper dig att snabbt förstå risken och vidta lämpliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-108">The recommendations are categorized across four focus areas, which help you quickly understand the risk and take action.</span></span>

<span data-ttu-id="1a3c5-109">Rekommendationerna baseras på kunskap och erfarenhet av Microsoft-tekniker från tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-109">The recommendations are based on the knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="1a3c5-110">Varje rekommendation innehåller information om varför ett problem kan verkligen är viktiga och hur du implementerar de föreslagna ändringarna.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-110">Each recommendation provides guidance about why an issue might matter to you and how to implement the suggested changes.</span></span>

<span data-ttu-id="1a3c5-111">Du kan välja fokusområden som är viktigast för din organisation och spåra din framsteg mot kör en risk ledigt och hälsosam miljö.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-111">You can choose focus areas that are most important to your organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="1a3c5-112">När du har lagt till lösningen och en bedömning är slutförd, Sammanfattning visas information för fokusområden på den **AD-bedömning** instrumentpanel för infrastrukturen i din miljö.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-112">After you've added the solution and an assessment is completed, summary information for focus areas is shown on the **AD Assessment** dashboard for the infrastructure in your environment.</span></span> <span data-ttu-id="1a3c5-113">I följande avsnitt beskrivs hur du använder informationen på den **AD-bedömning** instrumentpanel, där du kan visa och sedan vidta rekommenderade åtgärder för din serverinfrastruktur för Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-113">The following sections describe how to use the information on the **AD Assessment** dashboard, where you can view and then take recommended actions for your Active Directory server infrastructure.</span></span>

![Bild av SQL-bedömning sida vid sida](./media/log-analytics-ad-assessment/ad-tile.png)

![Bild av SQL-bedömning instrumentpanelen](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="1a3c5-116">Installera och konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="1a3c5-116">Installing and configuring the solution</span></span>
<span data-ttu-id="1a3c5-117">Använd följande information för att installera och konfigurera lösningarna.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-117">Use the following information to install and configure the solutions.</span></span>

* <span data-ttu-id="1a3c5-118">Agenterna måste installeras på domänkontrollanter som är medlemmar i domänen som ska utvärderas.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-118">Agents must be installed on domain controllers that are members of the domain to be evaluated.</span></span>
* <span data-ttu-id="1a3c5-119">Active Directory-bedömning lösningen kräver en version som stöds av .NET Framework 4 (4.5.2 eller senare) installerat på varje dator som har en OMS-agent.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-119">The Active Directory Assessment solution requires a supported version of .NET Framework 4 (4.5.2 or above) installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="1a3c5-120">Lägg till Active Directory-bedömning lösningen till OMS-arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) eller genom att använda processen som beskrivs i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="1a3c5-120">Add the Active Directory Assessment solution to your OMS workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>  <span data-ttu-id="1a3c5-121">Det krävs ingen ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-121">There is no further configuration required.</span></span>

  > [!NOTE]
  > <span data-ttu-id="1a3c5-122">När du har lagt till lösningen läggs filen AdvisorAssessment.exe till servrar med agenter.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-122">After you've added the solution, the AdvisorAssessment.exe file is added to servers with agents.</span></span> <span data-ttu-id="1a3c5-123">Konfigurationsdata läsa och sedan skickas till OMS-tjänsten i molnet för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-123">Configuration data is read and then sent to the OMS service in the cloud for processing.</span></span> <span data-ttu-id="1a3c5-124">Molntjänsten innehåller data att logik tillämpas för mottagna data.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-124">Logic is applied to the received data and the cloud service records the data.</span></span>
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a><span data-ttu-id="1a3c5-125">Active Directory Assessment samling information</span><span class="sxs-lookup"><span data-stu-id="1a3c5-125">Active Directory Assessment data collection details</span></span>

<span data-ttu-id="1a3c5-126">Active Directory-bedömning samlar in data från följande källor med agenter som du har aktiverat:</span><span class="sxs-lookup"><span data-stu-id="1a3c5-126">Active Directory Assessment collects data from the following sources using the agents that you have enabled:</span></span>

- <span data-ttu-id="1a3c5-127">Insamlare för registret</span><span class="sxs-lookup"><span data-stu-id="1a3c5-127">Registry collectors</span></span>
- <span data-ttu-id="1a3c5-128">LDAP-insamlare</span><span class="sxs-lookup"><span data-stu-id="1a3c5-128">LDAP collectors</span></span>
- <span data-ttu-id="1a3c5-129">.NET framework</span><span class="sxs-lookup"><span data-stu-id="1a3c5-129">.NET Framework</span></span>
- <span data-ttu-id="1a3c5-130">Händelsen logginsamlare</span><span class="sxs-lookup"><span data-stu-id="1a3c5-130">Event log collectors</span></span>
- <span data-ttu-id="1a3c5-131">Active Directory Service interfaces (ADSI)</span><span class="sxs-lookup"><span data-stu-id="1a3c5-131">Active Directory Service interfaces (ADSI)</span></span>
- <span data-ttu-id="1a3c5-132">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="1a3c5-132">Windows PowerShell</span></span>
- <span data-ttu-id="1a3c5-133">Filen datainsamlare</span><span class="sxs-lookup"><span data-stu-id="1a3c5-133">File data collectors</span></span>
- <span data-ttu-id="1a3c5-134">Windows Management Instrumentation (WMI)</span><span class="sxs-lookup"><span data-stu-id="1a3c5-134">Windows Management Instrumentation (WMI)</span></span>
- <span data-ttu-id="1a3c5-135">DCDIAG verktyget API</span><span class="sxs-lookup"><span data-stu-id="1a3c5-135">DCDIAG tool API</span></span>
- <span data-ttu-id="1a3c5-136">File Replication Service (NTFRS) API</span><span class="sxs-lookup"><span data-stu-id="1a3c5-136">File Replication Service (NTFRS) API</span></span>
- <span data-ttu-id="1a3c5-137">Anpassade C#-kod</span><span class="sxs-lookup"><span data-stu-id="1a3c5-137">Custom C# code</span></span>


<span data-ttu-id="1a3c5-138">I följande tabell visas data collection metoder för agenter, om Operations Manager (SCOM) krävs och hur ofta data samlas in av en agent.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-138">The following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="1a3c5-139">Plattform</span><span class="sxs-lookup"><span data-stu-id="1a3c5-139">platform</span></span> | <span data-ttu-id="1a3c5-140">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="1a3c5-140">Direct Agent</span></span> | <span data-ttu-id="1a3c5-141">SCOM-agent</span><span class="sxs-lookup"><span data-stu-id="1a3c5-141">SCOM agent</span></span> | <span data-ttu-id="1a3c5-142">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1a3c5-142">Azure Storage</span></span> | <span data-ttu-id="1a3c5-143">SCOM krävs?</span><span class="sxs-lookup"><span data-stu-id="1a3c5-143">SCOM required?</span></span> | <span data-ttu-id="1a3c5-144">SCOM-agent data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="1a3c5-144">SCOM agent data sent via management group</span></span> | <span data-ttu-id="1a3c5-145">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="1a3c5-145">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="1a3c5-146">Windows</span><span class="sxs-lookup"><span data-stu-id="1a3c5-146">Windows</span></span> |<span data-ttu-id="1a3c5-147">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a3c5-147">&#8226;</span></span> |<span data-ttu-id="1a3c5-148">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a3c5-148">&#8226;</span></span> |  |  |<span data-ttu-id="1a3c5-149">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a3c5-149">&#8226;</span></span> |<span data-ttu-id="1a3c5-150">7 dagar</span><span class="sxs-lookup"><span data-stu-id="1a3c5-150">7 days</span></span> |

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="1a3c5-151">Förstå hur rekommendationer prioriteras</span><span class="sxs-lookup"><span data-stu-id="1a3c5-151">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="1a3c5-152">Varje rekommendation är ett givet värde som identifierar en avvägning mellan kraven för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-152">Every recommendation made is given a weighting value that identifies the relative importance of the recommendation.</span></span> <span data-ttu-id="1a3c5-153">Endast de 10 viktigaste rekommendationerna visas.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-153">Only the 10 most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="1a3c5-154">Hur vikterna beräknas</span><span class="sxs-lookup"><span data-stu-id="1a3c5-154">How weights are calculated</span></span>
<span data-ttu-id="1a3c5-155">Viktningar är sammanställda värden som baseras på tre viktiga faktorer:</span><span class="sxs-lookup"><span data-stu-id="1a3c5-155">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="1a3c5-156">Den *sannolikhet* att ett problem som identifieras orsakar problem.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-156">The *probability* that an issue identified causes problems.</span></span> <span data-ttu-id="1a3c5-157">En högre sannolikhet är lika med en större övergripande poäng för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-157">A higher probability equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="1a3c5-158">Den *inverkan* av problemet i din organisation om det uppstår ett problem.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-158">The *impact* of the issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="1a3c5-159">En högre inverkan är lika med en större övergripande poäng för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-159">A higher impact equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="1a3c5-160">Den *arbete* krävs för att implementera denna rekommendation.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-160">The *effort* required to implement the recommendation.</span></span> <span data-ttu-id="1a3c5-161">Det är lika med en mindre övergripande poäng för rekommendationen högre prestanda.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-161">A higher effort equates to a smaller overall score for the recommendation.</span></span>

<span data-ttu-id="1a3c5-162">Värde för varje rekommendation uttrycks i procent av den sammanlagda poängen för varje fokusområde.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-162">The weighting for each recommendation is expressed as a percentage of the total score available for each focus area.</span></span> <span data-ttu-id="1a3c5-163">Till exempel ökar implementera denna rekommendation om en rekommendation i området för säkerhet och efterlevnad fokus har ett resultat på 5%, din övergripande säkerhet och efterlevnad poäng av 5%.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-163">For example, if a recommendation in the Security and Compliance focus area has a score of 5%, implementing that recommendation increases your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="1a3c5-164">Fokusområden</span><span class="sxs-lookup"><span data-stu-id="1a3c5-164">Focus areas</span></span>
<span data-ttu-id="1a3c5-165">**Säkerhet och efterlevnad** -fokus visas här rekommendationer för potentiella säkerhetshot och överträdelser, företagets principer och krav på teknisk, juridisk och regelmässig efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-165">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="1a3c5-166">**Tillgänglighet och affärskontinuitet** -fokus visas här rekommendationer för tjänstetillgänglighet, återhämtning för din infrastruktur och business-skydd.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-166">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="1a3c5-167">**Prestanda och skalbarhet** -fokus visas här rekommendationer som hjälper din organisations IT-infrastruktur växer, se till att din IT-miljö uppfyller aktuella prestandakrav och kan svara på förändrade infrastrukturbehov.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-167">**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.</span></span>

<span data-ttu-id="1a3c5-168">**Uppgradera migrering och distribution** -fokus visas här rekommendationer när du uppgraderar, migrera och distribuera Active Directory till din befintliga infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-168">**Upgrade, Migration and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy Active Directory to your existing infrastructure.</span></span>

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a><span data-ttu-id="1a3c5-169">Bör du försöka poängsätta 100% överallt fokus i?</span><span class="sxs-lookup"><span data-stu-id="1a3c5-169">Should you aim to score 100% in every focus area?</span></span>
<span data-ttu-id="1a3c5-170">Inte nödvändigtvis.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-170">Not necessarily.</span></span> <span data-ttu-id="1a3c5-171">Rekommendationerna baseras på kunskaper och erfarenheter som gjorts av Microsoft-tekniker över tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-171">The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="1a3c5-172">Dock inga två server-infrastrukturen är samma och specifika rekommendationer kan vara mer eller mindre relevant för dig.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-172">However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you.</span></span> <span data-ttu-id="1a3c5-173">Exempelvis kanske vissa säkerhetsrekommendationer mindre relevant om de virtuella datorerna inte utsätts för Internet.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-173">For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet.</span></span> <span data-ttu-id="1a3c5-174">Vissa tillgänglighet rekommendationer kan vara relevanta för tjänster som erbjuder med låg prioritet ad hoc-insamling och rapportering.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-174">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="1a3c5-175">Problem som är viktiga för en mogen företag kanske mindre viktiga för en Startup.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-175">Issues that are important to a mature business may be less important to a start-up.</span></span> <span data-ttu-id="1a3c5-176">Du kanske vill identifiera vilka fokusområden är dina prioriteter och titta på hur ditt resultat förändras över tid.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-176">You may want to identify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="1a3c5-177">Varje rekommendation innehåller information om varför det är viktigt.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-177">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="1a3c5-178">Du bör använda den här vägledningen för att utvärdera om implementera rekommendationen är lämplig för dig, baserat på typ av din IT-tjänster och affärsbehoven för din organisation.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-178">You should use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="1a3c5-179">Använd assessment fokus området rekommendationer</span><span class="sxs-lookup"><span data-stu-id="1a3c5-179">Use assessment focus area recommendations</span></span>
<span data-ttu-id="1a3c5-180">Innan du kan använda en lösning i OMS måste ha installerat lösningen.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-180">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="1a3c5-181">Du kan läsa mer om hur du installerar lösningar finns [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="1a3c5-181">To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="1a3c5-182">När den har installerats kan du visa sammanfattning av rekommendationer med hjälp av panelen Assessment på översiktssidan i OMS.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-182">After it is installed, you can view the summary of recommendations by using the Assessment tile on the Overview page in OMS.</span></span>

<span data-ttu-id="1a3c5-183">Visa sammanfattade efterlevnad bedömningar för din infrastruktur och gå till rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-183">View the summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="1a3c5-184">Visa rekommendationer för en Fokusområde och vidta åtgärder</span><span class="sxs-lookup"><span data-stu-id="1a3c5-184">To view recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="1a3c5-185">På den **översikt** klickar du på den **Assessment** panelen för din serverinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-185">On the **Overview** page, click the **Assessment** tile for your server infrastructure.</span></span>
2. <span data-ttu-id="1a3c5-186">På den **Assessment** , Granska sammanfattningen i ett fokus området blad och klickar sedan på en om du vill visa rekommendationer för området fokus.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-186">On the **Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.</span></span>
3. <span data-ttu-id="1a3c5-187">På någon av sidorna fokus område, kan du visa prioriterad rekommendationer för din miljö.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-187">On any of the focus area pages, you can view the prioritized recommendations made for your environment.</span></span> <span data-ttu-id="1a3c5-188">Klicka på en rekommendation enligt **påverkade objekt** att visa information om varför rekommendationen görs.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-188">Click a recommendation under **Affected Objects** to view details about why the recommendation is made.</span></span>  
    <span data-ttu-id="1a3c5-189">![Bild av Assessment rekommendationer](./media/log-analytics-ad-assessment/ad-focus.png)</span><span class="sxs-lookup"><span data-stu-id="1a3c5-189">![image of Assessment recommendations](./media/log-analytics-ad-assessment/ad-focus.png)</span></span>
4. <span data-ttu-id="1a3c5-190">Du kan vidta åtgärder i **föreslagna åtgärder**.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-190">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="1a3c5-191">När objektet har behandlats senare bedömningar poster som rekommenderade åtgärder som utförts och kompatibilitet poängen ökar.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-191">When the item has been addressed, later assessments records that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="1a3c5-192">Korrigerade objekt visas som **skickades objekt**.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-192">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="1a3c5-193">Ignorera rekommendationer</span><span class="sxs-lookup"><span data-stu-id="1a3c5-193">Ignore recommendations</span></span>
<span data-ttu-id="1a3c5-194">Om du har rekommendationer som du vill ignorera, kan du skapa en textfil som OMS använder för att förhindra rekommendationer visas i sökresultatet assessment.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-194">If you have recommendations that you want to ignore, you can create a text file that OMS will use to prevent recommendations from appearing in your assessment results.</span></span>

### <a name="to-identify-recommendations-that-you-will-ignore"></a><span data-ttu-id="1a3c5-195">Att identifiera rekommendationer som kommer att ignorera</span><span class="sxs-lookup"><span data-stu-id="1a3c5-195">To identify recommendations that you will ignore</span></span>
1. <span data-ttu-id="1a3c5-196">Logga in på din arbetsyta och öppna loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-196">Sign in to your workspace and open Log Search.</span></span> <span data-ttu-id="1a3c5-197">Använd följande fråga för att lista över rekommendationer som har misslyckats för datorer i din miljö.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-197">Use the following query to list recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> <span data-ttu-id="1a3c5-198">Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), sedan frågan ovan skulle ändra till följande.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-198">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   <span data-ttu-id="1a3c5-199">Här är en skärmbild som visar loggen sökfråga: ![misslyckades rekommendationer](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="1a3c5-199">Here's a screen shot showing the Log Search query: ![failed recommendations](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)</span></span>
2. <span data-ttu-id="1a3c5-200">Välj rekommendationer som du vill ignorera.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-200">Choose recommendations that you want to ignore.</span></span> <span data-ttu-id="1a3c5-201">Du ska använda värdena för RecommendationId i nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-201">You’ll use the values for RecommendationId in the next procedure.</span></span>

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="1a3c5-202">Du skapar och använder en IgnoreRecommendations.txt textfil</span><span class="sxs-lookup"><span data-stu-id="1a3c5-202">To create and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="1a3c5-203">Skapa en fil med namnet IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-203">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="1a3c5-204">Klistra in eller ange varje RecommendationId för varje rekommendation som du vill logganalys Ignorera på en separat rad och spara och stäng filen.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-204">Paste or type each RecommendationId for each recommendation that you want Log Analytics to ignore on a separate line and then save and close the file.</span></span>
3. <span data-ttu-id="1a3c5-205">Placera filen i följande mapp på varje dator där du vill att OMS att ignorera rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-205">Put the file in the following folder on each computer where you want OMS to ignore recommendations.</span></span>
   * <span data-ttu-id="1a3c5-206">På datorer med Microsoft Monitoring Agent (ansluten direkt eller via Operations Manager) - *SystemDrive*: \Program\Microsoft övervakning Agent\Agent</span><span class="sxs-lookup"><span data-stu-id="1a3c5-206">On computers with the Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="1a3c5-207">På hanteringsservern för Operations Manager - *SystemDrive*: \Program\Microsoft System Center 2012 R2\Operations Manager\Server</span><span class="sxs-lookup"><span data-stu-id="1a3c5-207">On the Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="to-verify-that-recommendations-are-ignored"></a><span data-ttu-id="1a3c5-208">Kontrollera att rekommendationer ignoreras</span><span class="sxs-lookup"><span data-stu-id="1a3c5-208">To verify that recommendations are ignored</span></span>
<span data-ttu-id="1a3c5-209">I nästa schemalagda utvärdering körs som standard var sjunde dag, de angivna rekommendationerna markeras *ignorerade* och visas inte på instrumentpanelen assessment.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-209">After the next scheduled assessment runs, by default every 7 days, the specified recommendations are marked *Ignored* and will not appear on the assessment dashboard.</span></span>

1. <span data-ttu-id="1a3c5-210">Du kan använda följande loggen sökfrågor för att lista alla rekommendationer som ignoreras.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-210">You can use the following Log Search queries to list all the ignored recommendations.</span></span>

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> <span data-ttu-id="1a3c5-211">Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), sedan frågan ovan skulle ändra till följande.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-211">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. <span data-ttu-id="1a3c5-212">Om du senare bestämmer att du vill se ignoreras rekommendationer, ta bort IgnoreRecommendations.txt filer eller du kan ta bort RecommendationIDs från dem.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-212">If you decide later that you want to see ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="ad-assessment-solutions-faq"></a><span data-ttu-id="1a3c5-213">Lösningar för AD-bedömning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="1a3c5-213">AD Assessment solutions FAQ</span></span>
<span data-ttu-id="1a3c5-214">*Hur ofta körs en utvärdering?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-214">*How often does an assessment run?*</span></span>

* <span data-ttu-id="1a3c5-215">Utvärderingen körs var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-215">The assessment runs every 7 days.</span></span>

<span data-ttu-id="1a3c5-216">*Finns det ett sätt att konfigurera hur ofta bedömning körs?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-216">*Is there a way to configure how often the assessment runs?*</span></span>

* <span data-ttu-id="1a3c5-217">Inte just nu.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-217">Not at this time.</span></span>

<span data-ttu-id="1a3c5-218">*Om en annan server för upptäcks när jag har lagt till en lösning, används den för att bedöma?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-218">*If another server for is discovered after I’ve added an assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="1a3c5-219">Ja, när det upptäcks att det bedöms från sedan, var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-219">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="1a3c5-220">*Om en server tas ur drift, när den tas bort från bedömningen?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-220">*If a server is decommissioned, when will it be removed from the assessment?*</span></span>

* <span data-ttu-id="1a3c5-221">Om en server inte lämnar data 3 veckor tas bort.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-221">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="1a3c5-222">*Vad är namnet på processen som gör datainsamlingen?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-222">*What is the name of the process that does the data collection?*</span></span>

* <span data-ttu-id="1a3c5-223">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="1a3c5-223">AdvisorAssessment.exe</span></span>

<span data-ttu-id="1a3c5-224">*Hur lång tid tar det för data som samlas in?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-224">*How long does it take for data to be collected?*</span></span>

* <span data-ttu-id="1a3c5-225">Samlingen faktiska data på servern tar ungefär en timme.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-225">The actual data collection on the server takes about 1 hour.</span></span> <span data-ttu-id="1a3c5-226">Det kan ta längre tid på servrar som har ett stort antal Active Directory-servrar.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-226">It may take longer on servers that have a large number of Active Directory servers.</span></span>

<span data-ttu-id="1a3c5-227">*Finns det ett sätt att konfigurera när data samlas in?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-227">*Is there a way to configure when data is collected?*</span></span>

* <span data-ttu-id="1a3c5-228">Inte just nu.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-228">Not at this time.</span></span>

<span data-ttu-id="1a3c5-229">*Varför visas endast topp 10 rekommendationer?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-229">*Why display only the top 10 recommendations?*</span></span>

* <span data-ttu-id="1a3c5-230">I stället för att ge dig en överväldigande uttömmande förteckning över aktiviteter, rekommenderar vi att du fokusera på adressering prioriterad rekommendationerna först.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-230">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing the prioritized recommendations first.</span></span> <span data-ttu-id="1a3c5-231">När du åtgärda dem blir ytterligare rekommendationer tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-231">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="1a3c5-232">Du kan visa alla rekommendationer loggen sökning om du vill se en detaljerad lista.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-232">If you prefer to see the detailed list, you can view all recommendations using Log Search.</span></span>

<span data-ttu-id="1a3c5-233">*Finns det ett sätt att ignorera en rekommendation?*</span><span class="sxs-lookup"><span data-stu-id="1a3c5-233">*Is there a way to ignore a recommendation?*</span></span>

* <span data-ttu-id="1a3c5-234">Ja, se [Ignorera rekommendationer](#ignore-recommendations) ovan.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-234">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a3c5-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a3c5-235">Next steps</span></span>
* <span data-ttu-id="1a3c5-236">Använd [logga sökningar i logganalys](log-analytics-log-searches.md) att visa detaljerade data för AD-bedömning och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="1a3c5-236">Use [Log searches in Log Analytics](log-analytics-log-searches.md) to view detailed AD Assessment data and recommendations.</span></span>
