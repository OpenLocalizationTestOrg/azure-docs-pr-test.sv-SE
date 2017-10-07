---
title: "aaaRemediate OS-säkerhetsproblem i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** reparera OS säkerhetsrisker **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 704103f7fb15835943d74b665d2bd56cb5e0a36d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a><span data-ttu-id="fc4eb-103">Åtgärda OS-säkerhetsproblem i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="fc4eb-103">Remediate OS vulnerabilities in Azure Security Center</span></span>
<span data-ttu-id="fc4eb-104">Azure Security Center analyserar dagligen virtuell dator (VM)-operativsystem (OS) för konfigurationer som kan göra hello VM mer sårbara tooattack och rekommenderar konfigurationsändringar tooaddress dessa problem.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-104">Azure Security Center analyzes daily your virtual machine (VM) operating system (OS) for configurations that could make hello VM more vulnerable tooattack and recommends configuration changes tooaddress these vulnerabilities.</span></span> <span data-ttu-id="fc4eb-105">Security Center rekommenderar att du löser säkerhetsproblem när den Virtuella datorns Operativsystemets konfiguration inte matchar hello rekommenderas konfigurationsregler.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-105">Security Center recommends that you resolve vulnerabilities when your VM’s OS configuration does not match hello recommended configuration rules.</span></span>

> [!NOTE]
> <span data-ttu-id="fc4eb-106">Mer information om hello specifika konfigurationer som övervakas finns hello [lista över rekommenderade konfigurationsregler](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="fc4eb-106">For more information on hello specific configurations being monitored, see hello [list of recommended configuration rules](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="fc4eb-107">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="fc4eb-107">Implement hello recommendation</span></span>

> [!NOTE]
> <span data-ttu-id="fc4eb-108">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="fc4eb-109">Det här dokumentet är inte en stegvis guide.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-109">This document is not a step-by-step guide.</span></span>
>
>

1. <span data-ttu-id="fc4eb-110">I hello **rekommendationer** bladet väljer **reparera OS säkerhetsrisker**.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-110">In hello **Recommendations** blade, select **Remediate OS vulnerabilities**.</span></span>
   <span data-ttu-id="fc4eb-111">![Åtgärda sårbarheter i operativsystem][1]</span><span class="sxs-lookup"><span data-stu-id="fc4eb-111">![Remediate OS vulnerabilities][1]</span></span>

    <span data-ttu-id="fc4eb-112">Hej **reparera OS säkerhetsrisker** blad öppnas och visar en lista över dina virtuella datorer med OS-konfigurationer som inte matchar hello rekommenderas konfigurationsregler.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-112">hello **Remediate OS vulnerabilities** blade opens and lists your VMs with OS configurations that do not match hello recommended configuration rules.</span></span>  <span data-ttu-id="fc4eb-113">För varje virtuell dator identifierar hello-bladet</span><span class="sxs-lookup"><span data-stu-id="fc4eb-113">For each VM, hello blade identifies:</span></span>

   * <span data-ttu-id="fc4eb-114">**Det gick inte regler** --hello antal regler som hello Virtuella datorns Operativsystemets konfiguration har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-114">**FAILED RULES** -- hello number of rules that hello VM's OS configuration failed.</span></span>
   * <span data-ttu-id="fc4eb-115">**SENASTE GENOMSÖKNINGSTID** --hello datum och tid att senast genomsöktes hello VM OS-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-115">**LAST SCAN TIME** -- hello date and time that Security Center last scanned hello VM’s OS configuration.</span></span>
   * <span data-ttu-id="fc4eb-116">**TILLSTÅND** --hello aktuell status för hello säkerhetsproblem:</span><span class="sxs-lookup"><span data-stu-id="fc4eb-116">**STATE** -- hello current state of hello vulnerability:</span></span>

     * <span data-ttu-id="fc4eb-117">Öppna: hello säkerhetsproblem har inte utförts än</span><span class="sxs-lookup"><span data-stu-id="fc4eb-117">Open: hello vulnerability has not been addressed yet</span></span>
     * <span data-ttu-id="fc4eb-118">Pågår: Hello säkerhetsproblem som tillämpas för närvarande, ingen åtgärd krävs av du</span><span class="sxs-lookup"><span data-stu-id="fc4eb-118">In Progress: hello vulnerability is currently being applied, no action is required by you</span></span>
     * <span data-ttu-id="fc4eb-119">Matcha: hello säkerhetsproblem har redan lösts (när hello problemet har åtgärdats hello-posten är nedtonad)</span><span class="sxs-lookup"><span data-stu-id="fc4eb-119">Resolved: hello vulnerability was already addressed (when hello issue has been resolved, hello entry is grayed out)</span></span>
   * <span data-ttu-id="fc4eb-120">**ALLVARLIGHETSGRAD** --alla sårbarheter ställs tooa allvarlighetsgraden låg, vilket innebär en säkerhetsrisk som bör åtgärdas men inte kräver omedelbara uppmärksamhet.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-120">**SEVERITY** -- All vulnerabilities are set tooa severity of Low, meaning a vulnerability should be addressed but does not require immediate attention.</span></span>

2. <span data-ttu-id="fc4eb-121">Välj en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-121">Select a VM.</span></span> <span data-ttu-id="fc4eb-122">Ett blad för den virtuella datorn öppnas och visar hello regler som har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-122">A blade for that VM opens and displays hello rules that have failed.</span></span>
   <span data-ttu-id="fc4eb-123">![Konfigurationsregler som har misslyckats][2]</span><span class="sxs-lookup"><span data-stu-id="fc4eb-123">![Configuration rules that have failed][2]</span></span>

3. <span data-ttu-id="fc4eb-124">Välj en regel.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-124">Select a rule.</span></span> <span data-ttu-id="fc4eb-125">I det här exemplet kan du markera **lösenord måste uppfylla krav på komplexitet**.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-125">In this example, lets select **Password must meet complexity requirements**.</span></span> <span data-ttu-id="fc4eb-126">Då öppnas ett blad som beskriver hello misslyckades regeln och hello effekt.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-126">A blade opens describing hello failed rule and hello impact.</span></span> <span data-ttu-id="fc4eb-127">Granska hello information och Överväg hur operativsystemkonfigurationerna tillämpas.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-127">Review hello details and consider how operating system configurations are applied.</span></span>
  <span data-ttu-id="fc4eb-128">![Beskrivning för hello misslyckades regel][3]</span><span class="sxs-lookup"><span data-stu-id="fc4eb-128">![Description for hello failed rule][3]</span></span>

  <span data-ttu-id="fc4eb-129">Security Center använder Common Configuration Enumeration (CCE) tooassign unika identifierare för konfigurationsregler.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-129">Security Center uses Common Configuration Enumeration (CCE) tooassign unique identifiers for configuration rules.</span></span> <span data-ttu-id="fc4eb-130">hello följande information finns på det här bladet:</span><span class="sxs-lookup"><span data-stu-id="fc4eb-130">hello following information is provided on this blade:</span></span>

  - <span data-ttu-id="fc4eb-131">NAMN – Namnet på regel</span><span class="sxs-lookup"><span data-stu-id="fc4eb-131">NAME -- Name of rule</span></span>
  - <span data-ttu-id="fc4eb-132">--CCE allvarlighetsgrad allvarlighetsgrad Kritisk, viktigt eller varning</span><span class="sxs-lookup"><span data-stu-id="fc4eb-132">SEVERITY -- CCE severity value of critical, important, or warning</span></span>
  - <span data-ttu-id="fc4eb-133">CCIED--CCE Unik identifierare för hello regel</span><span class="sxs-lookup"><span data-stu-id="fc4eb-133">CCIED -- CCE unique identifier for hello rule</span></span>
  - <span data-ttu-id="fc4eb-134">Beskrivning – Beskrivning av regeln</span><span class="sxs-lookup"><span data-stu-id="fc4eb-134">DESCRIPTION -- Description of rule</span></span>
  - <span data-ttu-id="fc4eb-135">SÄKERHETSPROBLEM--Förklaring av säkerhetsproblem eller risk om regeln inte är installerat</span><span class="sxs-lookup"><span data-stu-id="fc4eb-135">VULNERABILITY -- Explanation of vulnerability or risk if rule is not applied</span></span>
  - <span data-ttu-id="fc4eb-136">PÅVERKAN--Påverkan på verksamheten när regeln har tillämpats</span><span class="sxs-lookup"><span data-stu-id="fc4eb-136">IMPACT -- Business impact when rule is applied</span></span>
  - <span data-ttu-id="fc4eb-137">FÖRVÄNTAT värde--Värde förväntades när Security Center analyserar VM OS konfigurationen mot hello regel</span><span class="sxs-lookup"><span data-stu-id="fc4eb-137">EXPECTED VALUE -- Value expected when Security Center analyzes your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="fc4eb-138">--REGELN regeln åtgärden används av Security Center under analys av VM OS konfigurationen mot hello regel</span><span class="sxs-lookup"><span data-stu-id="fc4eb-138">RULE OPERATION -- Rule operation used by Security Center during analysis of your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="fc4eb-139">FAKTISKT värde - Värde returnerades efter analys av VM OS konfigurationen mot hello regel</span><span class="sxs-lookup"><span data-stu-id="fc4eb-139">ACTUAL VALUE -- Value returned after analysis of your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="fc4eb-140">UTVÄRDERINGSRESULTAT av –-resultatet av analysen: skicka, misslyckas</span><span class="sxs-lookup"><span data-stu-id="fc4eb-140">EVALUATION RESULT –- Result of analysis: Pass, Fail</span></span>

## <a name="see-also"></a><span data-ttu-id="fc4eb-141">Se även</span><span class="sxs-lookup"><span data-stu-id="fc4eb-141">See also</span></span>
<span data-ttu-id="fc4eb-142">Den här artikeln visar hur hello tooimplement Security Center rekommendation ”reparera OS säkerhetsproblem”.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-142">This article showed you how tooimplement hello Security Center recommendation "Remediate OS vulnerabilities."</span></span> <span data-ttu-id="fc4eb-143">Du kan granska hello uppsättning konfigurationsregler [här](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="fc4eb-143">You can review hello set of configuration rules [here](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span> <span data-ttu-id="fc4eb-144">Security Center använder CCE (Common Configuration uppräkning) tooassign unika identifierare för konfigurationsregler.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-144">Security Center uses CCE (Common Configuration Enumeration) tooassign unique identifiers for configuration rules.</span></span> <span data-ttu-id="fc4eb-145">Besök hello [CCE](https://nvd.nist.gov/cce/index.cfm) webbplats för mer information.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-145">Visit hello [CCE](https://nvd.nist.gov/cce/index.cfm) site for more information.</span></span>

<span data-ttu-id="fc4eb-146">toolearn mer om Security Center finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="fc4eb-146">toolearn more about Security Center, see hello following resources:</span></span>

* <span data-ttu-id="fc4eb-147">[Plattformar som stöds i Azure Security Center](security-center-os-coverage.md) -visar en lista över Windows och Linux virtuella datorer som stöds.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-147">[Supported platforms in Azure Security Center](security-center-os-coverage.md) - Provides a list of supported Windows and Linux VMs.</span></span>
* <span data-ttu-id="fc4eb-148">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-148">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="fc4eb-149">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-149">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="fc4eb-150">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-150">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="fc4eb-151">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-151">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="fc4eb-152">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-152">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) - Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="fc4eb-153">[Vanliga frågor om Azure Security Center](security-center-faq.md) -finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-153">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="fc4eb-154">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) -hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="fc4eb-154">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
