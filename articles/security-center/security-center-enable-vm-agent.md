---
title: Aktivera VM-agenten i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur du implementerar rekommenderar Azure Security Center ** aktivera VM Agent **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 337a7adfd93c76882a749685702bea6d1524c96a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a><span data-ttu-id="18013-103">Aktivera VM-agenten i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="18013-103">Enable VM Agent in Azure Security Center</span></span>
<span data-ttu-id="18013-104">VM-agenten måste installeras på virtuella datorer (VM) för att [Aktivera datainsamling](security-center-enable-data-collection.md).</span><span class="sxs-lookup"><span data-stu-id="18013-104">The VM Agent must be installed on virtual machines (VMs) in order to [enable data collection](security-center-enable-data-collection.md).</span></span>  <span data-ttu-id="18013-105">Azure Security Center kan du se vilka virtuella datorer som kräver den Virtuella Datoragenten och rekommenderar att du aktiverar VM-agenten på dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="18013-105">Azure Security Center enables you to see which VMs require the VM Agent and will recommend that you enable the VM Agent on those VMs.</span></span>

<span data-ttu-id="18013-106">VM-agenten installeras som standard för virtuella datorer som distribueras från Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="18013-106">The VM Agent is installed by default for VMs that are deployed from the Azure Marketplace.</span></span> <span data-ttu-id="18013-107">Artikeln [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) (VM-agenter och tillägg – del 2) innehåller information om hur VM-agenten ska installeras.</span><span class="sxs-lookup"><span data-stu-id="18013-107">The article [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) provides information on how to install the VM Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="18013-108">I det här dokumentet beskrivs tjänsten genom en exempeldistribution.</span><span class="sxs-lookup"><span data-stu-id="18013-108">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="18013-109">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="18013-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="18013-110">Implementera rekommendationen</span><span class="sxs-lookup"><span data-stu-id="18013-110">Implement the recommendation</span></span>
1. <span data-ttu-id="18013-111">I den **rekommendationer bladet**väljer **aktivera VM-agenten**.</span><span class="sxs-lookup"><span data-stu-id="18013-111">In the **Recommendations blade**, select **Enable VM Agent**.</span></span>
   <span data-ttu-id="18013-112">![Aktivera VM-Agent][1]</span><span class="sxs-lookup"><span data-stu-id="18013-112">![Enable VM Agent][1]</span></span>
2. <span data-ttu-id="18013-113">Gör det öppnas bladet **VM-agenten saknas eller inte svarar**.</span><span class="sxs-lookup"><span data-stu-id="18013-113">This opens the blade **VM Agent Is Missing Or Not Responding**.</span></span> <span data-ttu-id="18013-114">Det här bladet visar de virtuella datorer som kräver VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="18013-114">This blade lists the VMs that require the VM Agent.</span></span> <span data-ttu-id="18013-115">Följ instruktionerna på bladet för att installera den Virtuella datoragenten.</span><span class="sxs-lookup"><span data-stu-id="18013-115">Follow the instructions on the blade to install the VM agent.</span></span>
   <span data-ttu-id="18013-116">![VM-agenten saknas][2]</span><span class="sxs-lookup"><span data-stu-id="18013-116">![VM Agent is missing][2]</span></span>

## <a name="see-also"></a><span data-ttu-id="18013-117">Se även</span><span class="sxs-lookup"><span data-stu-id="18013-117">See also</span></span>
<span data-ttu-id="18013-118">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="18013-118">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="18013-119">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md): Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="18013-119">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="18013-120">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md): Här får du lära dig hur du kan skydda resurserna i Azure med hjälp av rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="18013-120">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="18013-121">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md): Här kan du läsa om hur du övervakar dina Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="18013-121">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="18013-122">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md): Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="18013-122">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="18013-123">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md): Här får du lära dig hur övervakar dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="18013-123">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="18013-124">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md): Här finns vanliga frågor om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18013-124">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="18013-125">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här kan du hitta de senaste nyheterna och aktuell information om säkerheten i Azure .</span><span class="sxs-lookup"><span data-stu-id="18013-125">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
