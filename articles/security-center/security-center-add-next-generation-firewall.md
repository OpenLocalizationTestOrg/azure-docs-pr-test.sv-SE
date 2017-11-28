---
title: "aaaAdd nästa generations brandvägg i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** lägga till en nästa generations brandvägg ** och ** väg traffice via nästa generations Brandvägg endast **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9a80f12571ba08eadf3361728c6321388c863235
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a><span data-ttu-id="97718-103">Lägg till nästa generations brandvägg i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="97718-103">Add a Next Generation Firewall in Azure Security Center</span></span>
<span data-ttu-id="97718-104">Azure Security Center kan rekommenderar att du lägger till nästa generations brandvägg (nästa generations Brandvägg) från en Microsoft-partner tooincrease din säkerhetsskydd.</span><span class="sxs-lookup"><span data-stu-id="97718-104">Azure Security Center may recommend that you add a next generation firewall (NGFW) from a Microsoft partner tooincrease your security protections.</span></span> <span data-ttu-id="97718-105">Det här dokumentet vägleder dig genom ett exempel på hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="97718-105">This document walks you through an example of how toodo this.</span></span>

> [!NOTE]
> <span data-ttu-id="97718-106">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="97718-106">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="97718-107">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="97718-107">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="97718-108">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="97718-108">Implement hello recommendation</span></span>
1. <span data-ttu-id="97718-109">I hello **rekommendationer** bladet väljer **lägger du till nästa generations brandvägg**.</span><span class="sxs-lookup"><span data-stu-id="97718-109">In hello **Recommendations** blade, select **Add a Next Generation Firewall**.</span></span>
   <span data-ttu-id="97718-110">![Lägga till en nästa generations brandvägg][1]</span><span class="sxs-lookup"><span data-stu-id="97718-110">![Add a Next Generation Firewall][1]</span></span>
2. <span data-ttu-id="97718-111">I hello **lägger du till nästa generations brandvägg** bladet väljer du en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="97718-111">In hello **Add a Next Generation Firewall** blade, select an endpoint.</span></span>
   <span data-ttu-id="97718-112">![Välj en slutpunkt][2]</span><span class="sxs-lookup"><span data-stu-id="97718-112">![Select an endpoint][2]</span></span>
3. <span data-ttu-id="97718-113">En andra **lägger du till nästa generations brandvägg** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="97718-113">A second **Add a Next Generation Firewall** blade opens.</span></span> <span data-ttu-id="97718-114">Du kan välja toouse en befintlig lösning om det är tillgängligt eller du kan skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="97718-114">You can choose toouse an existing solution if available or you can create a new one.</span></span> <span data-ttu-id="97718-115">I det här exemplet finns det inga befintliga lösningar så skapar vi ett nästa generations Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="97718-115">In this example, there are no existing solutions available so we create an NGFW.</span></span>
   <span data-ttu-id="97718-116">![Skapa nästa generations brandvägg][3]</span><span class="sxs-lookup"><span data-stu-id="97718-116">![Create Next Generation Firewall][3]</span></span>
4. <span data-ttu-id="97718-117">toocreate en nästa generations Brandvägg, markera en lösning hello listan med integrerade partnerleverantörer.</span><span class="sxs-lookup"><span data-stu-id="97718-117">toocreate an NGFW, select a solution from hello list of integrated partners.</span></span> <span data-ttu-id="97718-118">I det här exemplet väljer vi **Check Point**.</span><span class="sxs-lookup"><span data-stu-id="97718-118">In this example, we select **Check Point**.</span></span>
   <span data-ttu-id="97718-119">![Välj nästa generations brandvägg lösning][4]</span><span class="sxs-lookup"><span data-stu-id="97718-119">![Select Next Generation Firewall solution][4]</span></span>
5. <span data-ttu-id="97718-120">Hej **Check Point** öppnas ett blad med information om hello partnerlösning.</span><span class="sxs-lookup"><span data-stu-id="97718-120">hello **Check Point** blade opens providing you information about hello partner solution.</span></span> <span data-ttu-id="97718-121">Välj **skapa** hello information-bladet.</span><span class="sxs-lookup"><span data-stu-id="97718-121">Select **Create** in hello information blade.</span></span>
   <span data-ttu-id="97718-122">![Brandväggen information bladet][5]</span><span class="sxs-lookup"><span data-stu-id="97718-122">![Firewall information blade][5]</span></span>
6. <span data-ttu-id="97718-123">Hej **Skapa virtuell dator** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="97718-123">hello **Create virtual machine** blade opens.</span></span> <span data-ttu-id="97718-124">Här kan du ange information som krävs toospin av en virtuell dator (VM) som kör hello nästa generations Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="97718-124">Here you can enter information required toospin up a virtual machine (VM) that runs hello NGFW.</span></span> <span data-ttu-id="97718-125">Följ hello steg och ange hello nästa generations Brandvägg information som krävs.</span><span class="sxs-lookup"><span data-stu-id="97718-125">Follow hello steps and provide hello NGFW information required.</span></span> <span data-ttu-id="97718-126">Välj OK tooapply.</span><span class="sxs-lookup"><span data-stu-id="97718-126">Select OK tooapply.</span></span>
   <span data-ttu-id="97718-127">![Skapa virtuella toorun nästa generations Brandvägg][6]</span><span class="sxs-lookup"><span data-stu-id="97718-127">![Create virtual machine toorun NGFW][6]</span></span>

## <a name="route-traffic-through-ngfw-only"></a><span data-ttu-id="97718-128">Vidarebefordra trafik enbart via NGFW</span><span class="sxs-lookup"><span data-stu-id="97718-128">Route traffic through NGFW only</span></span>
<span data-ttu-id="97718-129">Returnera toohello **rekommendationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="97718-129">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="97718-130">En ny post skapades när du har lagt till en nästa generations Brandvägg via Security Center, som kallas **dirigera trafik via nästa generations Brandvägg endast**.</span><span class="sxs-lookup"><span data-stu-id="97718-130">A new entry was generated after you added an NGFW via Security Center, called **Route traffic through NGFW only**.</span></span> <span data-ttu-id="97718-131">Denna rekommendation skapas bara om du har installerat din nästa generations Brandvägg via Security Center.</span><span class="sxs-lookup"><span data-stu-id="97718-131">This recommendation is created only if you installed your NGFW through Security Center.</span></span> <span data-ttu-id="97718-132">Om du har mot Internet slutpunkter rekommenderar Security Center att du konfigurerar Network Security Group regler som tvingar inkommande trafik tooyour VM via din nästa generations Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="97718-132">If you have Internet-facing endpoints, Security Center recommends that you configure Network Security Group rules that force inbound traffic tooyour VM through your NGFW.</span></span>

1. <span data-ttu-id="97718-133">I hello **rekommendationer bladet**väljer **dirigera trafik via nästa generations Brandvägg endast**.</span><span class="sxs-lookup"><span data-stu-id="97718-133">In hello **Recommendations blade**, select **Route traffic through NGFW only**.</span></span>
   <span data-ttu-id="97718-134">![Dirigera trafiken endast via NGFW][7]</span><span class="sxs-lookup"><span data-stu-id="97718-134">![Route traffic through NGFW only][7]</span></span>
2. <span data-ttu-id="97718-135">Då öppnas bladet hello **dirigera trafik via nästa generations Brandvägg endast**, som visar en lista över virtuella datorer som du kan vidarebefordra trafiken till.</span><span class="sxs-lookup"><span data-stu-id="97718-135">This opens hello blade **Route traffic through NGFW only**, which lists VMs that you can route traffic to.</span></span> <span data-ttu-id="97718-136">Välj en virtuell dator hello-listan.</span><span class="sxs-lookup"><span data-stu-id="97718-136">Select a VM from hello list.</span></span>
   <span data-ttu-id="97718-137">![Välj en virtuell dator][8]</span><span class="sxs-lookup"><span data-stu-id="97718-137">![Select a VM][8]</span></span>
3. <span data-ttu-id="97718-138">Ett blad för hello valt VM öppnas Visa relaterade regler för inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="97718-138">A blade for hello selected VM opens, displaying related inbound rules.</span></span> <span data-ttu-id="97718-139">En beskrivning får du mer information om möjliga nästa steg.</span><span class="sxs-lookup"><span data-stu-id="97718-139">A description provides you with more information on possible next steps.</span></span> <span data-ttu-id="97718-140">Välj **redigera regler för inkommande trafik** tooproceed redigera en inkommande regel.</span><span class="sxs-lookup"><span data-stu-id="97718-140">Select **Edit inbound rules** tooproceed with editing an inbound rule.</span></span> <span data-ttu-id="97718-141">hello förväntningen är att **källa** har inte angetts för**alla** för hello mot Internet slutpunkter som är kopplad till hello nästa generations Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="97718-141">hello expectation is that **Source** is not set too**Any** for hello Internet-facing endpoints linked with hello NGFW.</span></span> <span data-ttu-id="97718-142">toolearn mer om hello egenskaperna för hello inkommande regel, se [NSG-regler](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="97718-142">toolearn more about hello properties of hello inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>
   <span data-ttu-id="97718-143">![Konfigurera regler toolimit åtkomst][9]
   ![Redigera inkommande regel][10]</span><span class="sxs-lookup"><span data-stu-id="97718-143">![Configure rules toolimit access][9]
![Edit inbound rule][10]</span></span>

## <a name="see-also"></a><span data-ttu-id="97718-144">Se även</span><span class="sxs-lookup"><span data-stu-id="97718-144">See also</span></span>
<span data-ttu-id="97718-145">Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Lägg till nästa generations brandvägg”.</span><span class="sxs-lookup"><span data-stu-id="97718-145">This document showed you how tooimplement hello Security Center recommendation "Add a Next Generation Firewall."</span></span> <span data-ttu-id="97718-146">toolearn mer om NGFWs och hello Check Point partnerlösning finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="97718-146">toolearn more about NGFWs and hello Check Point partner solution, see hello following:</span></span>

* [<span data-ttu-id="97718-147">Nästa generations brandvägg</span><span class="sxs-lookup"><span data-stu-id="97718-147">Next-Generation Firewall</span></span>](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [<span data-ttu-id="97718-148">Check Point vSEC</span><span class="sxs-lookup"><span data-stu-id="97718-148">Check Point vSEC</span></span>](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

<span data-ttu-id="97718-149">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="97718-149">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="97718-150">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="97718-150">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies.</span></span>
* <span data-ttu-id="97718-151">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="97718-151">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="97718-152">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="97718-152">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="97718-153">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="97718-153">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="97718-154">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="97718-154">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="97718-155">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="97718-155">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="97718-156">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="97718-156">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
