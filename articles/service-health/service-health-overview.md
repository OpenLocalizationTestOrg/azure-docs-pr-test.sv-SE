---
title: "aaaAzure tjänstens hälsa översikt | Microsoft Docs"
description: "Anpassad information om hur dina Azure appar påverkas av problem med aktuella och framtida Azure-tjänsten och underhåll."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.openlocfilehash: 2b536ee2f19757d4f2baf5529866c3d159a4670c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-health"></a>Azure Service Health
Tjänstens hälsa för Azure ger lämplig och personlig information när problem i Azure-tjänster påverkar dina tjänster.  Det hjälper dig också för kommande planerat underhåll.

## <a name="service-health-events"></a>Hälsa av tjänsten-händelser
Tjänstens hälsa spårar tre typer av hälsotillstånd händelser som kan påverka dina resurser:
1. **Tjänsten problem** -problem i hello Azure-tjänster som påverkar dig just nu. 
2. **Planerat underhåll** -kommande underhåll som kan påverka hello tillgänglighet i framtida hello-tjänster.  
3. **Hälsotillstånd rekommendationerna** -ändringar i Azure-tjänster som kräver din uppmärksamhet. Exempel är när Azure-funktioner är inaktuella eller om du överskrider en kvot.

    ![Händelser för Hälsotjänst](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a>Kom igång med tjänstens hälsa
toolaunch tjänstens hälsa för instrumentpanelen, väljer hello tjänstens hälsa panelen på instrumentpanelen i portalen. Om du tidigare har tagit bort hello panelen eller om du använder anpassade instrumentpanel, söka efter tjänstens hälsa för tjänsten i ”fler tjänster” (nedre vänstra på instrumentpanelen).
![Kom igång med tjänstens hälsa](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Visa aktuella problem som påverkar dina tjänster
Hej **tjänsten problem** pågående problem visas i Azure-tjänster som som påverkar dina resurser. Du kan förstå när hello problemet började och vilka tjänster och regioner som påverkas. Du kan också läsa hello senaste uppdatering toounderstand vad Azure gör tooresolve hello problemet. 
![Hantera serviceproblem](./media/service-health-overview/azure-service-health-overview-2.png)

Välj hello **potentiella påverkan** fliken toosee hello specifik lista med resurser som du äger och som kan påverkas av problemet hello. Du kan hämta en CSV-lista över dessa resurser tooshare med din grupp.
![Hantera problem med tjänsten - påverkan](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Hämta länkar och nedladdningsbara förklaringar 
Du kan hämta en länk för hello problemet toouse i systemet för hantering av problemet. Du kan hämta PDF och ibland tooshare för CSV-filer med personer som inte har åtkomst toohello Azure-portalen.   
![Hantera problem med tjänsten - problemhantering](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Få support från Microsoft
Kontakta supporten om resursen finns kvar i ett dåligt tillstånd även efter hello problemet är löst.  Länkarna hello stöd på hello höger i hello-sidan.  

## <a name="pin-a-personalized-health-map-tooyour-dashboard"></a>Fästa en anpassad hälsa kartan tooyour instrumentpanel
Filtrera tjänstens hälsa tooshow din verksamhetskritiska prenumerationer, regioner och resurstyper. Spara hello filter och PIN-kod en anpassad world kartan tooyour portal instrumentpanelen för hälsotillstånd. 
![Filter anpassade hälsa kartan](./media/service-health-overview/azure-service-health-overview-6a.png)
![fästa en anpassad hälsa karta](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Konfigurera aviseringar för tjänstens hälsa
Tjänstens hälsa för Azure integreras med Azure-Monitor tooalert du via e-post och textmeddelanden webhook meddelanden när resurserna verksamhetskritiska påverkas. Konfigurera en varning för logg för hello tjänstens hälsa för händelsen. Väg som varnar toohello rätt personer i organisationen med hjälp av åtgärdsgrupper. Mer information finns i [konfigurera aviseringar för tjänstens hälsa](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)

# <a name="next-steps"></a>Nästa steg
Ställa in aviseringar så meddelas du om hälsotillstånd. Mer information finns i [konfigurera aviseringar för tjänstens hälsa](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 