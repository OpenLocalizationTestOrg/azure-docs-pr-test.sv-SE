---
title: aaaEnable VM-agenten i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera VM Agent **."
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
ms.openlocfilehash: 9bd71e638b020780537da25fd4cf7baf34d3e11a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a><span data-ttu-id="07640-103">Aktivera VM-agenten i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="07640-103">Enable VM Agent in Azure Security Center</span></span>
<span data-ttu-id="07640-104">hello VM-agenten måste installeras på virtuella datorer (VM) i ordning för[Aktivera datainsamling](security-center-enable-data-collection.md).</span><span class="sxs-lookup"><span data-stu-id="07640-104">hello VM Agent must be installed on virtual machines (VMs) in order too[enable data collection](security-center-enable-data-collection.md).</span></span>  <span data-ttu-id="07640-105">Azure Security Center aktiverar du toosee som virtuella datorer kräver hello VM-agenten och rekommenderar att du aktiverar hello VM-agenten på dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="07640-105">Azure Security Center enables you toosee which VMs require hello VM Agent and will recommend that you enable hello VM Agent on those VMs.</span></span>

<span data-ttu-id="07640-106">hello VM-agenten installeras som standard för virtuella datorer som distribueras från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="07640-106">hello VM Agent is installed by default for VMs that are deployed from hello Azure Marketplace.</span></span> <span data-ttu-id="07640-107">hello artikel [VM-Agent och tillägg – del 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) innehåller information om hur tooinstall hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="07640-107">hello article [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) provides information on how tooinstall hello VM Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="07640-108">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="07640-108">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="07640-109">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="07640-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="07640-110">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="07640-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="07640-111">I hello **rekommendationer bladet**väljer **aktivera VM-agenten**.</span><span class="sxs-lookup"><span data-stu-id="07640-111">In hello **Recommendations blade**, select **Enable VM Agent**.</span></span>
   <span data-ttu-id="07640-112">![Aktivera VM-Agent][1]</span><span class="sxs-lookup"><span data-stu-id="07640-112">![Enable VM Agent][1]</span></span>
2. <span data-ttu-id="07640-113">Då öppnas bladet hello **VM-agenten saknas eller inte svarar**.</span><span class="sxs-lookup"><span data-stu-id="07640-113">This opens hello blade **VM Agent Is Missing Or Not Responding**.</span></span> <span data-ttu-id="07640-114">Det här bladet visar hello virtuella datorer som kräver hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="07640-114">This blade lists hello VMs that require hello VM Agent.</span></span> <span data-ttu-id="07640-115">Följ instruktionerna för hello på hello bladet tooinstall hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="07640-115">Follow hello instructions on hello blade tooinstall hello VM agent.</span></span>
   <span data-ttu-id="07640-116">![VM-agenten saknas][2]</span><span class="sxs-lookup"><span data-stu-id="07640-116">![VM Agent is missing][2]</span></span>

## <a name="see-also"></a><span data-ttu-id="07640-117">Se även</span><span class="sxs-lookup"><span data-stu-id="07640-117">See also</span></span>
<span data-ttu-id="07640-118">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="07640-118">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="07640-119">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md)– Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="07640-119">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="07640-120">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md): Här får du lära dig hur du kan skydda resurserna i Azure med hjälp av rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="07640-120">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="07640-121">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md)– Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="07640-121">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="07640-122">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)– Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="07640-122">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="07640-123">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="07640-123">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="07640-124">[Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="07640-124">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="07640-125">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/)--hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="07640-125">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
