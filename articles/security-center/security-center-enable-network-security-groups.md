---
title: "aaaEnable Nätverkssäkerhetsgrupper i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera Network Security grupper **."
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
ms.openlocfilehash: 2f70fe432aa452f833a5c322d13102ebbd6dbb69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a><span data-ttu-id="6e647-103">Aktivera Nätverkssäkerhetsgrupper i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="6e647-103">Enable Network Security Groups in Azure Security Center</span></span>
<span data-ttu-id="6e647-104">Azure Security Center rekommenderar att du aktiverar en nätverkssäkerhetsgrupp (NSG) om en inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="6e647-104">Azure Security Center recommends that you enable a network security group (NSG) if one is not already enabled.</span></span> <span data-ttu-id="6e647-105">NSG: er innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar nätverkstrafik tooyour VM-instanser i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="6e647-105">NSGs contain a list of Access Control List (ACL) rules that allow or deny network traffic tooyour VM instances in a Virtual Network.</span></span> <span data-ttu-id="6e647-106">NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet.</span><span class="sxs-lookup"><span data-stu-id="6e647-106">NSGs can be associated with either subnets or individual VM instances within that subnet.</span></span> <span data-ttu-id="6e647-107">När en NSG är associerad med ett undernät, gäller hello ACL-regler tooall hello VM-instanser i det undernätet.</span><span class="sxs-lookup"><span data-stu-id="6e647-107">When an NSG is associated with a subnet, hello ACL rules apply tooall hello VM instances in that subnet.</span></span> <span data-ttu-id="6e647-108">Dessutom kan trafik tooan enskild VM begränsas ytterligare genom att koppla en NSG direkt toothat VM.</span><span class="sxs-lookup"><span data-stu-id="6e647-108">In addition, traffic tooan individual VM can be restricted further by associating an NSG directly toothat VM.</span></span> <span data-ttu-id="6e647-109">Det finns fler toolearn [vad är en Nätverkssäkerhetsgrupp (NSG)?](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="6e647-109">toolearn more see [What is a Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)</span></span>

<span data-ttu-id="6e647-110">Om du inte har NSG: er aktiverat Security Center visas två rekommendationer tooyou: aktivera Nätverkssäkerhetsgrupper i undernät och aktivera Nätverkssäkerhetsgrupper på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6e647-110">If you do not have NSGs enabled, Security Center presents two recommendations tooyou: Enable Network Security Groups on subnets and Enable Network Security Groups on virtual machines.</span></span> <span data-ttu-id="6e647-111">Du kan välja vilken nivå, undernät eller Virtuella tooapply NSG: er.</span><span class="sxs-lookup"><span data-stu-id="6e647-111">You choose which level, subnet or VM, tooapply NSGs.</span></span>

> [!NOTE]
> <span data-ttu-id="6e647-112">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="6e647-112">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="6e647-113">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="6e647-113">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="6e647-114">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="6e647-114">Implement hello recommendation</span></span>
1. <span data-ttu-id="6e647-115">I hello **rekommendationer** bladet väljer **aktivera Nätverkssäkerhetsgrupper** på undernät eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6e647-115">In hello **Recommendations** blade, select **Enable Network Security Groups** on subnets or on virtual machines.</span></span>
   <span data-ttu-id="6e647-116">![Aktivera nätverkssäkerhetsgrupper][1]</span><span class="sxs-lookup"><span data-stu-id="6e647-116">![Enable Network Security Groups][1]</span></span>
2. <span data-ttu-id="6e647-117">Då öppnas bladet hello **konfigurera saknade Nätverkssäkerhetsgrupper** för undernät eller virtuella datorer, beroende på hello rekommenderar att du har valt.</span><span class="sxs-lookup"><span data-stu-id="6e647-117">This opens hello blade **Configure Missing Network Security Groups** for subnets or for virtual machines, depending on hello recommendation that you selected.</span></span> <span data-ttu-id="6e647-118">Välj ett undernät eller en virtuell dator tooconfigure en NSG på.</span><span class="sxs-lookup"><span data-stu-id="6e647-118">Select a subnet or a virtual machine tooconfigure an NSG on.</span></span>

   ![Konfigurera NSG för undernätet][2]

   ![Konfigurera NSG för VM][3]
3. <span data-ttu-id="6e647-121">På hello **Välj nätverkssäkerhetsgrupp** bladet Välj en befintlig NSG eller **Skapa nytt** toocreate en NSG.</span><span class="sxs-lookup"><span data-stu-id="6e647-121">On hello **Choose network security group** blade, select an existing NSG or select **Create new** toocreate an NSG.</span></span>

   ![Välj Nätverkssäkerhetsgrupp][4]

<span data-ttu-id="6e647-123">Om du skapar en NSG åtgärderna hello i [hur toomanage NSG: er med hjälp av hello Azure-portalen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate en NSG och ange säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="6e647-123">If you create an NSG, follow hello steps in [How toomanage NSGs using hello Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate an NSG and set security rules.</span></span>

## <a name="see-also"></a><span data-ttu-id="6e647-124">Se även</span><span class="sxs-lookup"><span data-stu-id="6e647-124">See also</span></span>
<span data-ttu-id="6e647-125">Den här artikeln visar hur hello tooimplement Security Center rekommendation ”aktivera Nätverkssäkerhetsgrupper” för undernät eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6e647-125">This article showed you how tooimplement hello Security Center recommendation "Enable Network Security Groups" for subnets or virtual machines.</span></span> <span data-ttu-id="6e647-126">toolearn mer om hur du aktiverar NSG: er, se hello följande:</span><span class="sxs-lookup"><span data-stu-id="6e647-126">toolearn more about enabling NSGs, see hello following:</span></span>

* [<span data-ttu-id="6e647-127">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="6e647-127">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="6e647-128">Hur toomanage NSG: er med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6e647-128">How toomanage NSGs using hello Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="6e647-129">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="6e647-129">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="6e647-130">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="6e647-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="6e647-131">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="6e647-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="6e647-132">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="6e647-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="6e647-133">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="6e647-133">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="6e647-134">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="6e647-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="6e647-135">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6e647-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="6e647-136">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="6e647-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
