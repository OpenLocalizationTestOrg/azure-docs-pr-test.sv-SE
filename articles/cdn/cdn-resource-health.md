---
title: "aaaMonitor hello hälsotillståndet för Azure CDN resurser | Microsoft Docs"
description: "Lär dig hur toomonitor hello hälsotillståndet för dina Azure CDN-resurser med hjälp av Azure Resource Health."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0a77e56d2fecae4bde6c83730c05375853a6638a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hello-health-of-azure-cdn-resources"></a><span data-ttu-id="59399-103">Övervaka hello Azure CDN-resursers hälsa</span><span class="sxs-lookup"><span data-stu-id="59399-103">Monitor hello health of Azure CDN resources</span></span>
  
<span data-ttu-id="59399-104">Azure CDN Resource health är en delmängd av [Azure resurshälsa](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59399-104">Azure CDN Resource health is a subset of [Azure resource health](../resource-health/resource-health-overview.md).</span></span>  <span data-ttu-id="59399-105">Du kan använda Azure resource health toomonitor hello CDN-resursers hälsa och få tillämplig vägledning tootroubleshoot problem.</span><span class="sxs-lookup"><span data-stu-id="59399-105">You can use Azure resource health toomonitor hello health of CDN resources and receive actionable guidance tootroubleshoot problems.</span></span>

>[!IMPORTANT] 
><span data-ttu-id="59399-106">Azure CDN-resurshälsa svarar bara för närvarande för hello hälsotillståndet för leverans av CDN-globala och API-funktioner.</span><span class="sxs-lookup"><span data-stu-id="59399-106">Azure CDN resource health only currently accounts for hello health of global CDN delivery and API capabilities.</span></span>  <span data-ttu-id="59399-107">Azure CDN-resurshälsa verifierar inte enskilda CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="59399-107">Azure CDN resource health does not verify individual CDN endpoints.</span></span>
>
><span data-ttu-id="59399-108">hello signaler som matas Azure CDN resurshälsa kanske in too15 minuter försenad.</span><span class="sxs-lookup"><span data-stu-id="59399-108">hello signals that feed Azure CDN resource health may be up too15 minutes delayed.</span></span>

## <a name="how-toofind-azure-cdn-resource-health"></a><span data-ttu-id="59399-109">Hur toofind Azure CDN resurshälsa</span><span class="sxs-lookup"><span data-stu-id="59399-109">How toofind Azure CDN resource health</span></span>

1. <span data-ttu-id="59399-110">I hello [Azure-portalen](https://portal.azure.com), bläddra tooyour CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="59399-110">In hello [Azure portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>

2. <span data-ttu-id="59399-111">Klicka på hello **inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="59399-111">Click hello **Settings** button.</span></span>

    ![Inställningar för](./media/cdn-resource-health/cdn-profile-settings.png)

3. <span data-ttu-id="59399-113">Under *stöd + felsökning*, klickar du på **Resource health**.</span><span class="sxs-lookup"><span data-stu-id="59399-113">Under *Support + troubleshooting*, click **Resource health**.</span></span>

    ![CDN-resurshälsa](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
><span data-ttu-id="59399-115">Du kan också hitta CDN-resurserna som visas i hello *resurshälsa* panelen i hello *hjälp + support* bladet.</span><span class="sxs-lookup"><span data-stu-id="59399-115">You can also find CDN resources listed in hello *Resource health* tile in hello *Help + support* blade.</span></span>  <span data-ttu-id="59399-116">Du kan snabbt få för*hjälp + support* genom att klicka på hello inringad **?**</span><span class="sxs-lookup"><span data-stu-id="59399-116">You can quickly get too*Help + support* by clicking hello circled **?**</span></span> <span data-ttu-id="59399-117">i hello övre högra hörnet av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="59399-117">in hello upper right corner of hello portal.</span></span>
>
> ![Hjälp + support](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a><span data-ttu-id="59399-119">Azure CDN-specifika meddelanden</span><span class="sxs-lookup"><span data-stu-id="59399-119">Azure CDN-specific messages</span></span>

<span data-ttu-id="59399-120">Statusvärden relaterade tooAzure CDN resource health finns nedan.</span><span class="sxs-lookup"><span data-stu-id="59399-120">Statuses related tooAzure CDN resource health can be found below.</span></span>

|<span data-ttu-id="59399-121">Meddelande</span><span class="sxs-lookup"><span data-stu-id="59399-121">Message</span></span> | <span data-ttu-id="59399-122">Rekommenderad åtgärd</span><span class="sxs-lookup"><span data-stu-id="59399-122">Recommended Action</span></span> |
|---|---|
|<span data-ttu-id="59399-123">Om du har stoppats, tas bort eller felkonfigurerad en eller flera av dina CDN-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="59399-123">You may have stopped, removed, or misconfigured one or more of your CDN endpoints</span></span> | <span data-ttu-id="59399-124">Du kanske har stoppats, borttagen eller felkonfigurerad en eller flera av dina CDN-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="59399-124">You may have stopped, removed, or misconfigured one or more of your CDN endpoints.</span></span>|
|<span data-ttu-id="59399-125">Tyvärr, hello CDN management-tjänsten är inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="59399-125">We are sorry, hello CDN management service is currently unavailable</span></span> | <span data-ttu-id="59399-126">Återkom för statusuppdateringar; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid.</span><span class="sxs-lookup"><span data-stu-id="59399-126">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span>|
|<span data-ttu-id="59399-127">Tyvärr CDN-slutpunkter kan påverkas av pågående problem med några av våra CDN-providers</span><span class="sxs-lookup"><span data-stu-id="59399-127">We're sorry, your CDN endpoints may be impacted by ongoing issues with some of our CDN providers</span></span> | <span data-ttu-id="59399-128">Återkom för statusuppdateringar; Använd hello felsöka verktyget toolearn hur tootest din ursprung och CDN-slutpunkten; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid.</span><span class="sxs-lookup"><span data-stu-id="59399-128">Check back here for status updates; Use hello Troubleshoot tool toolearn how tootest your origin and CDN endpoint; If your problem persists after hello expected resolution time, contact support.</span></span> |
|<span data-ttu-id="59399-129">Tyvärr konfigurationsändringar för CDN-slutpunkten fördröjs spridning</span><span class="sxs-lookup"><span data-stu-id="59399-129">We're sorry, CDN endpoint configuration changes are experiencing propagation delays</span></span> | <span data-ttu-id="59399-130">Återkom för statusuppdateringar; Kontakta supporten om konfigurationsändringarna inte sprids helt i hello förväntad tid.</span><span class="sxs-lookup"><span data-stu-id="59399-130">Check back here for status updates; If your configuration changes are not fully propagated in hello expected time, contact support.</span></span>|
|<span data-ttu-id="59399-131">Vi beklagar, men vi har problem läser in hello kompletterande portalen</span><span class="sxs-lookup"><span data-stu-id="59399-131">We're sorry, we are experiencing issues loading hello supplemental portal</span></span> | <span data-ttu-id="59399-132">Återkom för statusuppdateringar; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid.</span><span class="sxs-lookup"><span data-stu-id="59399-132">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span>|
<span data-ttu-id="59399-133">Tyvärr vi har problem med några av våra CDN-providers</span><span class="sxs-lookup"><span data-stu-id="59399-133">We are sorry, we are experiencing issues with some of our CDN providers</span></span> | <span data-ttu-id="59399-134">Återkom för statusuppdateringar; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid.</span><span class="sxs-lookup"><span data-stu-id="59399-134">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="59399-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59399-135">Next steps</span></span>

- [<span data-ttu-id="59399-136">Läs en översikt över Azure resurshälsa</span><span class="sxs-lookup"><span data-stu-id="59399-136">Read an overview of Azure resource health</span></span>](../resource-health/resource-health-overview.md)
- [<span data-ttu-id="59399-137">Felsökning av problem med CDN komprimering</span><span class="sxs-lookup"><span data-stu-id="59399-137">Troubleshoot issues with CDN compression</span></span>](./cdn-troubleshoot-compression.md)
- [<span data-ttu-id="59399-138">Felsöka problem med 404-fel</span><span class="sxs-lookup"><span data-stu-id="59399-138">Troubleshoot issues with 404 errors</span></span>](./cdn-troubleshoot-endpoint.md)