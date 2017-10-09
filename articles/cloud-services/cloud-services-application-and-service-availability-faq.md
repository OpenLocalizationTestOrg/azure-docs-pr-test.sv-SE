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
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Program- och problem med tjänsters tillgänglighet för Azure Cloud Services: vanliga frågor (FAQ)

Den här artikeln innehåller vanliga frågor och svar om programmet och problem med tjänsters tillgänglighet för [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Du kan också läsa hello [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Min roll har återvunnits. Har det skett någon uppdatering distribuerat för tjänst i molnet?
Ungefär släpper en gång i månaden, Microsoft en ny gäst-OS-version för Windows Azure PaaS virtuella datorer. Gäst-OS är endast en sådan uppdatering i hello pipeline. En version kan påverkas av flera faktorer. Dessutom körs Azure på hundratals tusentals datorer. Därför är det omöjligt toopredict hello exakt datum och tid när rollerna kommer att startas om. Vi uppdaterar hello gäst OS uppdatera RSS-Feed med hello senaste informationen som vi har men du bör överväga som rapporterats tid toobe ungefärligt värde. Vi vet att det här är problematisk för kunderna och arbetar med en plan toolimit eller exakt tid omstarter.

Mer information om de senaste uppdateringarna för Gästoperativsystem, finns i [Azure gäst-OS-versioner och SDK-kompatibilitetsmatris](cloud-services-guestos-update-matrix.md).

Praktisk information om startas om och pekare tootechnical information om uppdateringar med gästen och Värdoperativsystem finns hello MSDN bloggposten [roll-instansen startas om på grund av tooOS uppgraderingar](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).

## <a name="why-does-hello-first-request-toomy-cloud-service-after-hello-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Varför hello första begäran toomy molntjänst när hello-tjänsten har varit inaktiv under en viss tid tar det längre tid än vanligt?
När hello webbservern tar emot hello första begäran först kompilerar hello koden och bearbetar sedan dessa hello begäran. Som är varför hello första begäran tar längre tid än hello andra. Som standard hämtar hello programpoolen stängs av i fall av användaren inaktivitet. hello programpoolen kommer också att återanvändas som standard var 1,740 minuter (29 timmar).

Internet Information Services (IIS)-programpooler kan vara regelbundet återvunna tooavoid instabilt tillstånd som kan leda tooapplication krascher, låser sig eller minnesläckor.

följande dokument hello hjälper dig att förstå och undvika det här problemet:
* [Åtgärda långsam första last för IIS](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [IIS 7.5 webb programmet första förfrågan efter-programpool återvinning långsamt](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

Om du vill toochange hello standardbeteendet för IIS, behöver du toouse Start uppgifter, eftersom om du tillämpar ändringar toohello Webbroll instanser manuellt, hello ändringar så småningom att gå förlorade.

Mer information finns i [hur tooconfigure och kör startade aktiviteter för en molntjänst](cloud-services-startup-tasks.md).
