---
title: "Distributionsproblem för Microsoft Azure Cloud Services FAQ | Microsoft Docs"
description: "Den här artikeln innehåller vanliga frågor om Microsoft Azure Cloud Services-distribution."
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
ms.openlocfilehash: cb62cd4c4635d9e837dff81a9c4543781dd11adb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Distributionsproblem för Azure Cloud Services: vanliga frågor (FAQ)

Den här artikeln innehåller vanliga frågor och svar om distributionsproblem för [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Du kan också kontakta den [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Varför distribuerar en tjänst i molnet till mellanlagringsplatsen Ibland misslyckas med en resurs Allokeringsfel om det finns redan en befintlig distribution på produktionsplatsen?
Om en tjänst i molnet har en distribution i antingen uttag, är hela Molntjänsten fäst på ett specifikt kluster. Det innebär att om det finns redan en distribution på produktionsplatsen är en ny fristående distribution kan endast tilldelas i samma kluster som produktionsplatsen.

Allokeringsfel inträffa när klustret där Molntjänsten finns inte har tillräckligt med fysiska beräkningsresurser för att uppfylla din distributionsbegäran om.

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Varför skala upp eller skala ut en tjänst molndistribution ibland medföra Allokeringsfel?
När en molnbaserad tjänst har distribuerats, hämtar vanligtvis fäster du det ett specifikt kluster. Det innebär att skala upp eller ut en befintlig molntjänst måste tilldelas nya instanser i samma kluster. Om klustret närmar sig kapacitetsgränsen eller önskade VM-storlek/typen är inte tillgänglig, misslyckas begäran.

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Varför distribuerar en tjänst i molnet till en tillhörighetsgrupp ibland resulterar i Allokeringsfel?
En ny distribution till en tom molntjänst kan allokeras av infrastrukturresurser i ett kluster i den regionen, såvida inte Molntjänsten är fäst på en tillhörighetsgrupp. Distributioner till samma tillhörighetsgrupp görs på samma kluster. Om klustret närmar sig kapacitetsgränsen, misslyckas begäran.

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Varför ändra VM-storlek eller lägga till en ny virtuell dator till en befintlig molntjänst ibland resulterar i Allokeringsfel?
Kluster i ett datacenter kan ha olika konfigurationer för datortyper (t.ex. en serie, Av2 serie, D-serien, Dv2-serien, G-serien, H serien osv.). Men inte alla kluster måste nödvändigtvis alla typer av virtuella datorer. Till exempel om du försöker lägga till en D-serien VM till en molntjänst som redan har distribuerats i ett kluster som en serie endast får en Allokeringsfel. Detta kommer också hända om du försöker ändra VM SKU storlekar (till exempel växlar från en A-series till D-serien).

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

Du kan kontrollera storlekarna som finns i din region [Microsoft Azure: produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Varför distribuerar en tjänst i molnet senare misslyckas på grund av begränsningar/kvoter/villkor för min prenumeration eller tjänst?
Distribution av en molnbaserad tjänst kan misslyckas om de resurser som krävs för att allokera överskrider standard eller maxkvoten som tillåts för tjänsten på nivån region/datacenter. Mer information finns i [molntjänster begränsar](../azure-subscription-service-limits.md#cloud-services-limits).

Du kan också spåra användning/kvoten för din prenumeration på portalen: Azure Portal = > prenumerationer = > \<lämpliga prenumeration > = > ”användning + kvot”.

Information om användning/förbrukning-relaterade resurser kan också hämtas via Azure Billing API: erna. Se [Azure Resursanvändning API (förhandsgranskning)](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Hur kan jag ändra storleken på en distribuerad molntjänst för virtuell dator utan att omdistribuera det?
Du kan inte ändra VM-storlek för en distribuerad molntjänst utan att omdistribuera den. VM-storlek är inbyggd i CSDEF som kan bara uppdateras med en ny distribution.

Mer information finns i [hur du uppdaterar en molntjänst](cloud-services-update-azure-service.md).

 
