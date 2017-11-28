---
title: "aaaRestrict åtkomst via Internet-riktade slutpunkter i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** begränsa åtkomst via Internetuppkopplad slutpunkt **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: ee72497088618d4db29b5ae4183f4fe77b498423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a><span data-ttu-id="cb8bc-103">Begränsa åtkomst via Internet-riktade slutpunkter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="cb8bc-103">Restrict access through Internet-facing endpoints in Azure Security Center</span></span>
<span data-ttu-id="cb8bc-104">Azure Security Center rekommenderar att du begränsar åtkomst via Internet-riktade slutpunkter om någon av dina Nätverkssäkerhetsgrupper (NSG: er) har en eller flera regler för inkommande trafik som tillåter åtkomst från ”alla” källans IP-adress.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-104">Azure Security Center will recommend that you restrict access through Internet-facing endpoints if any of your Network Security Groups (NSGs) has one or more inbound rules that allow access from “any” source IP address.</span></span> <span data-ttu-id="cb8bc-105">Öppna åtkomst för ”alla” kan aktivera angripare tooaccess dina resurser.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-105">Opening access too“any” may enable attackers tooaccess your resources.</span></span> <span data-ttu-id="cb8bc-106">Security Center rekommenderar att du redigerar regler för inkommande trafik toorestrict åtkomst toosource IP-adresserna som verkligen behöver åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-106">Security Center will recommend that you edit these inbound rules toorestrict access toosource IP addresses that actually need access.</span></span>

<span data-ttu-id="cb8bc-107">Denna rekommendation genereras för alla icke-web-portar som innehåller ”något” som källa.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-107">This recommendation is generated for any non-web port that has "any" as source.</span></span>

> [!NOTE]
> <span data-ttu-id="cb8bc-108">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-108">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="cb8bc-109">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="cb8bc-110">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="cb8bc-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="cb8bc-111">I hello **rekommendationer bladet**väljer **begränsa åtkomst via Internetuppkopplad slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-111">In hello **Recommendations blade**, select **Restrict access through Internet facing endpoint**.</span></span>

   ![Begränsa åtkomst via slutpunkt mot Internet][1]
2. <span data-ttu-id="cb8bc-113">Då öppnas bladet hello **begränsa åtkomst via Internetuppkopplad slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-113">This opens hello blade **Restrict access through Internet facing endpoint**.</span></span> <span data-ttu-id="cb8bc-114">Det här bladet visar hello virtuella datorer (VM) med regler för inkommande trafik som skapar en potentiell säkerhetsrisk.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-114">This blade lists hello virtual machines (VMs) with inbound rules that create a potential security issue.</span></span> <span data-ttu-id="cb8bc-115">Välj en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-115">Select a VM.</span></span>

   ![Välj en virtuell dator][2]
3. <span data-ttu-id="cb8bc-117">Hej **NSG** bladet innehåller Nätverkssäkerhetsgruppen, relaterade inkommande trafik och hello associerade VM.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-117">hello **NSG** blade displays Network Security Group information, related inbound rules, and hello associated VM.</span></span> <span data-ttu-id="cb8bc-118">Välj **redigera regler för inkommande trafik** tooproceed redigera en inkommande regel.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-118">Select **Edit inbound rules** tooproceed with editing an inbound rule.</span></span>

   ![Nätverkssäkerhetsgruppen bladet][3]
4. <span data-ttu-id="cb8bc-120">På hello **inkommande säkerhetsregler** bladet välj hello inkommande regel tooedit.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-120">On hello **Inbound security rules** blade select hello inbound rule tooedit.</span></span> <span data-ttu-id="cb8bc-121">I det här exemplet ska vi väljer **AllowWeb**.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-121">In this example, let’s select **AllowWeb**.</span></span>

   ![Inkommande säkerhetsregler][4]

   <span data-ttu-id="cb8bc-123">Observera att du kan också välja **standard regler** toosee hello uppsättning standardregler som finns i alla NSG: er.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-123">Note, you can also select **Default rules** toosee hello set of default rules contained by all NSGs.</span></span> <span data-ttu-id="cb8bc-124">hello standardreglerna kan inte tas bort, men eftersom de har tilldelats en lägre prioritet, de kan åsidosättas med hello regler som du skapar.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-124">hello default rules cannot be deleted but, because they are assigned a lower priority, they can be overridden by hello rules that you create.</span></span> <span data-ttu-id="cb8bc-125">Lär dig mer om [standard regler](../virtual-network/virtual-networks-nsg.md#default-rules).</span><span class="sxs-lookup"><span data-stu-id="cb8bc-125">Learn more about [default rules](../virtual-network/virtual-networks-nsg.md#default-rules).</span></span>

   ![Standardregler][5]
5. <span data-ttu-id="cb8bc-127">På hello **AllowWeb** bladet redigera hello egenskaperna för hello inkommande regel så som hello **källa** är ett IP-adress eller ett block av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-127">On hello **AllowWeb** blade, edit hello properties of hello inbound rule so that hello **Source** is an IP address or block of IP addresses.</span></span> <span data-ttu-id="cb8bc-128">toolearn mer om hello egenskaperna för hello inkommande regel, se [NSG-regler](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="cb8bc-128">toolearn more about hello properties of hello inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>

   ![Redigera regel för inkommande trafik][6]

## <a name="see-also"></a><span data-ttu-id="cb8bc-130">Se även</span><span class="sxs-lookup"><span data-stu-id="cb8bc-130">See also</span></span>
<span data-ttu-id="cb8bc-131">Den här artikeln visar hur hello tooimplement Security Center rekommendation ”begränsa åtkomst via Internetuppkopplad slutpunkt”.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-131">This article showed you how tooimplement hello Security Center recommendation "Restrict access through Internet facing endpoint.”</span></span> <span data-ttu-id="cb8bc-132">toolearn mer om hur du aktiverar NSG: er och regler, se hello följande:</span><span class="sxs-lookup"><span data-stu-id="cb8bc-132">toolearn more about enabling NSGs and rules, see hello following:</span></span>

* [<span data-ttu-id="cb8bc-133">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="cb8bc-133">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="cb8bc-134">Hur toomanage NSG: er med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cb8bc-134">How toomanage NSGs using hello Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="cb8bc-135">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="cb8bc-135">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="cb8bc-136">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md)– Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-136">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="cb8bc-137">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md): Här får du lära dig hur du kan skydda resurserna i Azure med hjälp av rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-137">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="cb8bc-138">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md)– Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-138">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="cb8bc-139">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)– Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-139">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="cb8bc-140">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-140">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="cb8bc-141">[Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-141">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="cb8bc-142">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/)--hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="cb8bc-142">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
