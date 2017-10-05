---
title: "Aktivera Nätverkssäkerhetsgrupper i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur du implementerar rekommenderar Azure Security Center ** aktivera Network Security grupper **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 1e034d59d8847f237fa0d4c772344d45cd618576
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a><span data-ttu-id="5986d-103">Aktivera Nätverkssäkerhetsgrupper i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="5986d-103">Enable Network Security Groups in Azure Security Center</span></span>
<span data-ttu-id="5986d-104">Azure Security Center rekommenderar att du aktiverar en nätverkssäkerhetsgrupp (NSG) om en inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="5986d-104">Azure Security Center recommends that you enable a network security group (NSG) if one is not already enabled.</span></span> <span data-ttu-id="5986d-105">NSG: er innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar nätverkstrafik till VM-instanser i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="5986d-105">NSGs contain a list of Access Control List (ACL) rules that allow or deny network traffic to your VM instances in a Virtual Network.</span></span> <span data-ttu-id="5986d-106">NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet.</span><span class="sxs-lookup"><span data-stu-id="5986d-106">NSGs can be associated with either subnets or individual VM instances within that subnet.</span></span> <span data-ttu-id="5986d-107">När en NSG är associerad med ett undernät, tillämpas ACL-reglerna på alla VM-instanser i det undernätet.</span><span class="sxs-lookup"><span data-stu-id="5986d-107">When an NSG is associated with a subnet, the ACL rules apply to all the VM instances in that subnet.</span></span> <span data-ttu-id="5986d-108">Dessutom kan trafik till en enskild VM begränsas ytterligare genom att koppla en NSG direkt till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5986d-108">In addition, traffic to an individual VM can be restricted further by associating an NSG directly to that VM.</span></span> <span data-ttu-id="5986d-109">Läs mer finns [vad är en Nätverkssäkerhetsgrupp (NSG)?](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="5986d-109">To learn more see [What is a Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)</span></span>

<span data-ttu-id="5986d-110">Om du inte har NSG: er aktiverat två rekommendationer för dig visas i Security Center: aktivera Nätverkssäkerhetsgrupper i undernät och aktivera Nätverkssäkerhetsgrupper på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5986d-110">If you do not have NSGs enabled, Security Center presents two recommendations to you: Enable Network Security Groups on subnets and Enable Network Security Groups on virtual machines.</span></span> <span data-ttu-id="5986d-111">Du kan välja vilka nivå, undernät eller virtuella datorn, för att tillämpa NSG: er.</span><span class="sxs-lookup"><span data-stu-id="5986d-111">You choose which level, subnet or VM, to apply NSGs.</span></span>

> [!NOTE]
> <span data-ttu-id="5986d-112">I det här dokumentet beskrivs tjänsten genom en exempeldistribution.</span><span class="sxs-lookup"><span data-stu-id="5986d-112">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="5986d-113">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="5986d-113">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="5986d-114">Implementera rekommendationen</span><span class="sxs-lookup"><span data-stu-id="5986d-114">Implement the recommendation</span></span>
1. <span data-ttu-id="5986d-115">I den **rekommendationer** bladet väljer **aktivera Nätverkssäkerhetsgrupper** på undernät eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5986d-115">In the **Recommendations** blade, select **Enable Network Security Groups** on subnets or on virtual machines.</span></span>
   <span data-ttu-id="5986d-116">![Aktivera nätverkssäkerhetsgrupper][1]</span><span class="sxs-lookup"><span data-stu-id="5986d-116">![Enable Network Security Groups][1]</span></span>
2. <span data-ttu-id="5986d-117">Gör det öppnas bladet **konfigurera saknade Nätverkssäkerhetsgrupper** för undernät eller virtuella datorer, beroende på rekommendationen som du har valt.</span><span class="sxs-lookup"><span data-stu-id="5986d-117">This opens the blade **Configure Missing Network Security Groups** for subnets or for virtual machines, depending on the recommendation that you selected.</span></span> <span data-ttu-id="5986d-118">Välj ett undernät eller en virtuell dator för att konfigurera en NSG på.</span><span class="sxs-lookup"><span data-stu-id="5986d-118">Select a subnet or a virtual machine to configure an NSG on.</span></span>

   ![Konfigurera NSG för undernätet][2]

   ![Konfigurera NSG för VM][3]
3. <span data-ttu-id="5986d-121">På den **Välj nätverkssäkerhetsgrupp** bladet Välj en befintlig NSG eller **Skapa nytt** att skapa en NSG.</span><span class="sxs-lookup"><span data-stu-id="5986d-121">On the **Choose network security group** blade, select an existing NSG or select **Create new** to create an NSG.</span></span>

   ![Välj Nätverkssäkerhetsgrupp][4]

<span data-ttu-id="5986d-123">Om du skapar en NSG, följer du stegen i [hantera NSG: er med hjälp av Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) att skapa en NSG och säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="5986d-123">If you create an NSG, follow the steps in [How to manage NSGs using the Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) to create an NSG and set security rules.</span></span>

## <a name="see-also"></a><span data-ttu-id="5986d-124">Se även</span><span class="sxs-lookup"><span data-stu-id="5986d-124">See also</span></span>
<span data-ttu-id="5986d-125">Den här artikeln visades hur du implementerar Security Center-rekommendationen ”aktivera Nätverkssäkerhetsgrupper” för undernät eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5986d-125">This article showed you how to implement the Security Center recommendation "Enable Network Security Groups" for subnets or virtual machines.</span></span> <span data-ttu-id="5986d-126">Mer information om hur du aktiverar NSG: er finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="5986d-126">To learn more about enabling NSGs, see the following:</span></span>

* [<span data-ttu-id="5986d-127">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="5986d-127">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="5986d-128">Hantera NSG: er med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="5986d-128">How to manage NSGs using the Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="5986d-129">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="5986d-129">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="5986d-130">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="5986d-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="5986d-131">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5986d-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="5986d-132">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig att övervaka hälsotillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5986d-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="5986d-133">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="5986d-133">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="5986d-134">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md): Här får du lära dig hur du övervakar dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="5986d-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="5986d-135">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5986d-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="5986d-136">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="5986d-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
