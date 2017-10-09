---
title: "hälsovarningar för aaaResolve endpoint protection i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** lösa Endpoint Protection hälsa aviseringar **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 9631d15aa1dfa9003d56332363ae7911061ed0b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a><span data-ttu-id="08885-103">Lös hälsovarningar för endpoint protection i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="08885-103">Resolve endpoint protection health alerts in Azure Security Center</span></span>
<span data-ttu-id="08885-104">Azure Security Center rekommenderar att du löser identifierade endpoint protection health-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="08885-104">Azure Security Center will recommend that you resolve detected endpoint protection health alerts.</span></span>  <span data-ttu-id="08885-105">Security Center kan du toosee vilka virtuella datorer (VM) har endpoint protection-fel och hur många fel.</span><span class="sxs-lookup"><span data-stu-id="08885-105">Security Center enables you toosee which virtual machines (VMs) have endpoint protection failures and how many failures.</span></span>

> [!NOTE]
> <span data-ttu-id="08885-106">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="08885-106">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="08885-107">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="08885-107">This is not a step-by-step guide.</span></span>
> 
> 

## <a name="implement-hello-recommendation"></a><span data-ttu-id="08885-108">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="08885-108">Implement hello recommendation</span></span>
1. <span data-ttu-id="08885-109">I hello **rekommendationer bladet**väljer **lösa Endpoint Protection hälsovarningar**.</span><span class="sxs-lookup"><span data-stu-id="08885-109">In hello **Recommendations blade**, select **Resolve Endpoint Protection health alerts**.</span></span>
   <span data-ttu-id="08885-110">![Lösa slutpunktsskydd för hälsovarningar][1]</span><span class="sxs-lookup"><span data-stu-id="08885-110">![Resolve endpoint protection health alerts][1]</span></span>
2. <span data-ttu-id="08885-111">Då öppnas bladet hello **Endpoint Protection-fel** som visar en lista över virtuella datorer med fel och hello antal fel för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="08885-111">This opens hello blade **Endpoint Protection failure** which lists VMs with failures and hello number of failures for each VM.</span></span> <span data-ttu-id="08885-112">Välj en virtuell dator hello-listan.</span><span class="sxs-lookup"><span data-stu-id="08885-112">Select a VM from hello list.</span></span>
   <span data-ttu-id="08885-113">![Endpoint protection-fel][2]</span><span class="sxs-lookup"><span data-stu-id="08885-113">![Endpoint protection failure][2]</span></span>
3. <span data-ttu-id="08885-114">En **fel listan** blad öppnas för hello valda VM, visas en lista över fel.</span><span class="sxs-lookup"><span data-stu-id="08885-114">A **Failures List** blade opens for hello selected VM, displaying a list of failures.</span></span> <span data-ttu-id="08885-115">Välj ett fel hello listan toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="08885-115">Select a failure from hello list toolearn more.</span></span> <span data-ttu-id="08885-116">Då öppnas ett blad med information om hello valt fel.</span><span class="sxs-lookup"><span data-stu-id="08885-116">This opens a blade with information about hello selected failure.</span></span>
   <span data-ttu-id="08885-117">![Fel listan][3]
   ![felhändelse][4]</span><span class="sxs-lookup"><span data-stu-id="08885-117">![Failures list][3]
![Failure event][4]</span></span>

## <a name="see-also"></a><span data-ttu-id="08885-118">Se även</span><span class="sxs-lookup"><span data-stu-id="08885-118">See also</span></span>
<span data-ttu-id="08885-119">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="08885-119">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="08885-120">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md)– Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="08885-120">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="08885-121">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md): Här får du lära dig hur du kan skydda resurserna i Azure med hjälp av rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="08885-121">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="08885-122">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md)– Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="08885-122">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="08885-123">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)– Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="08885-123">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="08885-124">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="08885-124">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="08885-125">[Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="08885-125">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="08885-126">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/)--hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="08885-126">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
