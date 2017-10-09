---
title: "aaaManaging partnerlösningar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet vägleder dig igenom hur Azure Security Center enkelt kan övervaka på en snabb överblick över hello hälsostatus på de partnerlösningar som är integrerade i azureprenumerationen."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: fc97aedf709b9044bfd3d4ecae0b58d5fa716bbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a><span data-ttu-id="697ea-103">Övervaka partnerlösningar med Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="697ea-103">Monitoring partner solutions with Azure Security Center</span></span>
<span data-ttu-id="697ea-104">Det här dokumentet vägleder dig igenom hur toomonitor hello hälsostatusen på partnerlösningar i Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="697ea-104">This document walks you through how toomonitor hello health status of your partner solutions in Azure Security Center.</span></span>

> [!NOTE]
> <span data-ttu-id="697ea-105">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="697ea-105">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="697ea-106">Det här dokumentet är inte en stegvis guide.</span><span class="sxs-lookup"><span data-stu-id="697ea-106">This document is not a step-by-step guide.</span></span>
>
>

## <a name="monitoring-partner-solutions"></a><span data-ttu-id="697ea-107">Övervaka partnerlösningar</span><span class="sxs-lookup"><span data-stu-id="697ea-107">Monitoring partner solutions</span></span>
<span data-ttu-id="697ea-108">Hej **partnerlösningar** panelen på hello **Security Center** bladet kan du snabbt en överblick över hello hälsostatus på de partnerlösningar som är integrerade i azureprenumerationen.</span><span class="sxs-lookup"><span data-stu-id="697ea-108">hello **Partner solutions** tile on hello **Security Center** blade lets you monitor at a glance hello health status of your partner solutions that are integrated with your Azure subscription.</span></span>

![Partnerlösningsrutan][1]

<span data-ttu-id="697ea-110">Hej **partnerlösningar** panelen visar hello antalet partnerlösningar som är integrerad med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="697ea-110">hello **Partner solutions** tile displays hello number of partner solutions integrated with your subscription.</span></span> <span data-ttu-id="697ea-111">Om det inte finns några lösningar integrerade, visar hello panelen hello siffran noll.</span><span class="sxs-lookup"><span data-stu-id="697ea-111">If there are no solutions integrated, hello tile displays hello number zero.</span></span>

<span data-ttu-id="697ea-112">tooview hello hälsa på partnerlösningarna:</span><span class="sxs-lookup"><span data-stu-id="697ea-112">tooview hello health of your partner solutions:</span></span>

1. <span data-ttu-id="697ea-113">Välj hello **partnerlösningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="697ea-113">Select hello **Partner solutions** tile.</span></span> <span data-ttu-id="697ea-114">Hej **partnerlösningar** blad öppnas visas en lista över partnerlösningarna anslutna tooSecurity Center.</span><span class="sxs-lookup"><span data-stu-id="697ea-114">hello **Partner solutions** blade opens displaying a list of your partner solutions connected tooSecurity Center.</span></span>

   ![Partnerlösningar][3]

   <span data-ttu-id="697ea-116">hello status för en partnerlösning kan vara:</span><span class="sxs-lookup"><span data-stu-id="697ea-116">hello status of a partner solution can be:</span></span>

   * <span data-ttu-id="697ea-117">Väl skyddad (grön): Det finns inga problem med säkerheten.</span><span class="sxs-lookup"><span data-stu-id="697ea-117">Protected (green) - there is no health issue.</span></span>
   * <span data-ttu-id="697ea-118">Inte väl skyddad (röd): Det finns säkerhetsproblem som måste åtgärdas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="697ea-118">Unhealthy (red) - there is a health issue that requires immediate attention.</span></span>
   * <span data-ttu-id="697ea-119">Stoppad reporting (orange): hello lösning har stoppats reporting dess hälsa.</span><span class="sxs-lookup"><span data-stu-id="697ea-119">Stopped reporting (orange) - hello solution has stopped reporting its health.</span></span>
   * <span data-ttu-id="697ea-120">Okänd skyddsstatus (orange): hello lösning hello hälsa är okänd just nu på grund av tooa misslyckades processen att lägga till en ny resurs toohello befintliga lösning.</span><span class="sxs-lookup"><span data-stu-id="697ea-120">Unknown protection status (orange) - hello health of hello solution is unknown at this time due tooa failed process of adding a new resource toohello existing solution.</span></span>
   * <span data-ttu-id="697ea-121">Inga rapporter (grå) - hello lösning har inte rapporterat något ännu, status för en lösning kan vara orapporterad om det nyligen har anslutits och fortfarande distribuera.</span><span class="sxs-lookup"><span data-stu-id="697ea-121">Not reported (gray) - hello solution has not reported anything yet, a solution's status may be unreported if it has recently been connected and is still deploying.</span></span>

2. <span data-ttu-id="697ea-122">Markera en partnerlösning.</span><span class="sxs-lookup"><span data-stu-id="697ea-122">Select a partner solution.</span></span> <span data-ttu-id="697ea-123">I det här exemplet kan du väljer hello **Qualys** lösning.</span><span class="sxs-lookup"><span data-stu-id="697ea-123">In this example, lets select hello **Qualys** solution.</span></span>  <span data-ttu-id="697ea-124">Då öppnas ett blad visar hello status för Partnerlösningen hello och hello lösning associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="697ea-124">A blade opens showing you hello status of hello partner solution and hello solution's associated resources.</span></span> <span data-ttu-id="697ea-125">Välj **lösning konsolen** tooopen hello partner partnerhanteringsfunktionen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="697ea-125">Select **Solution console** tooopen hello partner management experience for this solution.</span></span>

   ![Partnerlösningsinformation][4]
3. <span data-ttu-id="697ea-127">Gå tillbaka toohello **Qualys** och välj **länk VM**.</span><span class="sxs-lookup"><span data-stu-id="697ea-127">Go back toohello **Qualys** blade and select **Link VM**.</span></span> <span data-ttu-id="697ea-128">Hej **länken program** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="697ea-128">hello **Link Applications** blade opens.</span></span> <span data-ttu-id="697ea-129">Här kan du ansluta resurser toohello partnerlösning.</span><span class="sxs-lookup"><span data-stu-id="697ea-129">Here you can connect resources toohello partner solution.</span></span>

   ![Länken resurser toopartner lösning][5]

## <a name="next-steps"></a><span data-ttu-id="697ea-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="697ea-131">Next steps</span></span>
<span data-ttu-id="697ea-132">I detta dokument, som var introducerades toohello **partnerlösningar** i Security Center.</span><span class="sxs-lookup"><span data-stu-id="697ea-132">In this document, you were introduced toohello **Partner Solutions** tile in Security Center.</span></span> <span data-ttu-id="697ea-133">toolearn mer om Security Center finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="697ea-133">toolearn more about Security Center, see hello following articles:</span></span>

* <span data-ttu-id="697ea-134">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="697ea-134">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="697ea-135">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="697ea-135">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="697ea-136">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="697ea-136">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="697ea-137">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="697ea-137">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="697ea-138">[Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="697ea-138">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="697ea-139">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="697ea-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
