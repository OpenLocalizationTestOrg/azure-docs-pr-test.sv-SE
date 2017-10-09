---
title: "problem med aaaApplication och tjänsten tillgänglighet för Microsoft Azure Cloud Services FAQ | Microsoft Docs"
description: "Den här artikeln innehåller vanliga frågor om program och tjänstetillgänglighet för Microsoft Azure Cloud Services hello."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: cd0d9ba0beb1dc72d4761f8b89c2ecadb51c7e48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a><span data-ttu-id="a35e7-103">Program- och problem med tjänsters tillgänglighet för Azure Cloud Services: vanliga frågor (FAQ)</span><span class="sxs-lookup"><span data-stu-id="a35e7-103">Application and service availability issues for Azure Cloud Services: Frequently asked questions (FAQs)</span></span>

<span data-ttu-id="a35e7-104">Den här artikeln innehåller vanliga frågor och svar om programmet och problem med tjänsters tillgänglighet för [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span><span class="sxs-lookup"><span data-stu-id="a35e7-104">This article includes frequently asked questions about application and service availability issues for [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span></span> <span data-ttu-id="a35e7-105">Du kan också läsa hello [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.</span><span class="sxs-lookup"><span data-stu-id="a35e7-105">You can also consult hello [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a><span data-ttu-id="a35e7-106">Min roll har återvunnits.</span><span class="sxs-lookup"><span data-stu-id="a35e7-106">My role got recycled.</span></span> <span data-ttu-id="a35e7-107">Har det skett någon uppdatering distribuerat för tjänst i molnet?</span><span class="sxs-lookup"><span data-stu-id="a35e7-107">Was there any update rolled out for my cloud service?</span></span>
<span data-ttu-id="a35e7-108">Ungefär släpper en gång i månaden, Microsoft en ny gäst-OS-version för Windows Azure PaaS virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a35e7-108">Roughly once a month, Microsoft releases a new Guest OS version for Windows Azure PaaS VMs.</span></span><span data-ttu-id="a35e7-109"> Gäst-OS är endast en sådan uppdatering i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a35e7-109"> The Guest OS is only one such update in hello pipeline.</span></span> <span data-ttu-id="a35e7-110">En version kan påverkas av flera faktorer.</span><span class="sxs-lookup"><span data-stu-id="a35e7-110">A release can be affected by many other factors.</span></span> <span data-ttu-id="a35e7-111">Dessutom körs Azure på hundratals tusentals datorer.</span><span class="sxs-lookup"><span data-stu-id="a35e7-111">In addition, Azure runs on hundreds of thousands of machines.</span></span> <span data-ttu-id="a35e7-112">Därför är det omöjligt toopredict hello exakt datum och tid när rollerna kommer att startas om.</span><span class="sxs-lookup"><span data-stu-id="a35e7-112">Therefore, it's impossible toopredict hello exact date and time when your roles will reboot.</span></span> <span data-ttu-id="a35e7-113">Vi uppdaterar hello gäst OS uppdatera RSS-Feed med hello senaste informationen som vi har men du bör överväga som rapporterats tid toobe ungefärligt värde.</span><span class="sxs-lookup"><span data-stu-id="a35e7-113">We update hello Guest OS Update RSS Feed with hello latest information that we have, but you should consider that reported time toobe an approximate value.</span></span> <span data-ttu-id="a35e7-114">Vi vet att det här är problematisk för kunderna och arbetar med en plan toolimit eller exakt tid omstarter.</span><span class="sxs-lookup"><span data-stu-id="a35e7-114">We are aware that this is problematic for customers and are working on a plan toolimit or precisely time reboots.</span></span>

<span data-ttu-id="a35e7-115">Mer information om de senaste uppdateringarna för Gästoperativsystem, finns i [Azure gäst-OS-versioner och SDK-kompatibilitetsmatris](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="a35e7-115">For complete details about recent Guest OS updates, see [Azure Guest OS releases and SDK compatibility matrix](cloud-services-guestos-update-matrix.md).</span></span>

<span data-ttu-id="a35e7-116">Praktisk information om startas om och pekare tootechnical information om uppdateringar med gästen och Värdoperativsystem finns hello MSDN bloggposten [roll-instansen startas om på grund av tooOS uppgraderingar](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span><span class="sxs-lookup"><span data-stu-id="a35e7-116">For helpful information on restarts and pointers tootechnical details of Guest and Host OS updates, see hello MSDN blog post [Role Instance Restarts Due tooOS Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span></span>

## <a name="why-does-hello-first-request-toomy-cloud-service-after-hello-service-has-been-idle-for-some-time-take-longer-than-usual"></a><span data-ttu-id="a35e7-117">Varför hello första begäran toomy molntjänst när hello-tjänsten har varit inaktiv under en viss tid tar det längre tid än vanligt?</span><span class="sxs-lookup"><span data-stu-id="a35e7-117">Why does hello first request toomy cloud service after hello service has been idle for some time take longer than usual?</span></span>
<span data-ttu-id="a35e7-118">När hello webbservern tar emot hello första begäran först kompilerar hello koden och bearbetar sedan dessa hello begäran.</span><span class="sxs-lookup"><span data-stu-id="a35e7-118">When hello Web Server receives hello first request, it first recompiles hello code and then processes hello request.</span></span> <span data-ttu-id="a35e7-119">Som är varför hello första begäran tar längre tid än hello andra.</span><span class="sxs-lookup"><span data-stu-id="a35e7-119">That's why hello first request takes longer than hello others.</span></span> <span data-ttu-id="a35e7-120">Som standard hämtar hello programpoolen stängs av i fall av användaren inaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a35e7-120">By default, hello app pool gets shut down in cases of user inactivity.</span></span> <span data-ttu-id="a35e7-121">hello programpoolen kommer också att återanvändas som standard var 1,740 minuter (29 timmar).</span><span class="sxs-lookup"><span data-stu-id="a35e7-121">hello app pool will also recycle by default every 1,740 minutes (29 hours).</span></span>

<span data-ttu-id="a35e7-122">Internet Information Services (IIS)-programpooler kan vara regelbundet återvunna tooavoid instabilt tillstånd som kan leda tooapplication krascher, låser sig eller minnesläckor.</span><span class="sxs-lookup"><span data-stu-id="a35e7-122">Internet Information Services (IIS) application pools can be periodically recycled tooavoid unstable states that can lead tooapplication crashes, hangs, or memory leaks.</span></span>

<span data-ttu-id="a35e7-123">följande dokument hello hjälper dig att förstå och undvika det här problemet:</span><span class="sxs-lookup"><span data-stu-id="a35e7-123">hello following documents will help you understand and mitigate this issue:</span></span>
* [<span data-ttu-id="a35e7-124">Åtgärda långsam första last för IIS</span><span class="sxs-lookup"><span data-stu-id="a35e7-124">Fixing slow initial load for IIS</span></span>](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [<span data-ttu-id="a35e7-125">IIS 7.5 webb programmet första förfrågan efter-programpool återvinning långsamt</span><span class="sxs-lookup"><span data-stu-id="a35e7-125">IIS 7.5 web application first request after app-pool recycle very slow</span></span>](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

<span data-ttu-id="a35e7-126">Om du vill toochange hello standardbeteendet för IIS, behöver du toouse Start uppgifter, eftersom om du tillämpar ändringar toohello Webbroll instanser manuellt, hello ändringar så småningom att gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="a35e7-126">If you want toochange hello default behavior of IIS, you will need toouse startup tasks, because if you manually apply changes toohello Web Role instances, hello changes will eventually be lost.</span></span>

<span data-ttu-id="a35e7-127">Mer information finns i [hur tooconfigure och kör startade aktiviteter för en molntjänst](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="a35e7-127">For more information, see [How tooconfigure and run startup tasks for a cloud service](cloud-services-startup-tasks.md).</span></span>
