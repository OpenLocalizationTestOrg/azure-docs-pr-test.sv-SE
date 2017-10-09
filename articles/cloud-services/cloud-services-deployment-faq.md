---
title: "aaaDeployment problem för Microsoft Azure Cloud Services FAQ | Microsoft Docs"
description: "Den här artikeln visar hello vanliga frågor och svar om Microsoft Azure Cloud Services-distribution."
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
ms.openlocfilehash: 8d67e36aa87fb5794d358e5cc235123ac7286028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Distributionsproblem för Azure Cloud Services: vanliga frågor (FAQ)

Den här artikeln innehåller vanliga frågor och svar om distributionsproblem för [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Du kan också läsa hello [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-toohello-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-hello-production-slot"></a>Varför distribuerar en cloud service toohello mellanlagringsplatsen Ibland misslyckas med en resurs Allokeringsfel om det finns redan en befintlig distribution i hello produktionsplatsen?
Om en tjänst i molnet har en distribution i antingen uttag, är hello hela molntjänst Fäst tooa specifikt kluster. Det innebär att om det finns redan en distribution i hello produktionsplatsen, en ny fristående distribution kan endast allokeras i samma kluster som hello produktionsplatsen hello.

Allokeringsfel inträffa när hello kluster där Molntjänsten finns inte har tillräckligt med fysiskt och beräkna resurser toosatisfy distributionsbegäran.

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Varför skala upp eller skala ut en tjänst molndistribution ibland medföra Allokeringsfel?
När en molnbaserad tjänst har distribuerats, hämtar vanligtvis Fäst tooa specifikt kluster. Det innebär att skala upp/ut en befintlig cloud service måste tilldelas nya instanser i hello samma kluster. Om hello klustret närmar sig kapacitetsgränsen eller hello önskad VM storlek/typen är inte tillgänglig, hello begäran misslyckas.

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Varför distribuerar en tjänst i molnet till en tillhörighetsgrupp ibland resulterar i Allokeringsfel?
En ny distribution tooan tom molntjänst kan allokeras av hello fabric i ett kluster i den regionen, såvida inte hello molntjänst är fäst tooan tillhörighetsgrupp. Distributioner toohello samma tillhörighetsgrupp görs på hello samma kluster. Om klustret hello närmar sig kapacitetsgränsen, kan hello förfrågan misslyckas.

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-tooan-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Varför ändra VM-storlek eller lägga till en ny VM tooan befintlig molntjänst ibland resulterar i Allokeringsfel?
hello-kluster i ett datacenter kan ha olika konfigurationer för datortyper (t.ex. en serie, Av2 serie, D-serien, Dv2-serien, G-serien, H serien osv.). Men inte alla hello kluster måste nödvändigtvis alla hello typer av virtuella datorer. Till exempel om du försöker tooadd D-serien VM tooa tjänst i molnet som redan har distribuerats i ett kluster som en serie endast får en Allokeringsfel. Detta kan också inträffa om försök toochange VM SKU storlekar (till exempel växlar från en A-serien tooa D-serien).

Hur du lindra sådana Allokeringsfel finns [Molntjänsten Allokeringsfel: lösningar](cloud-services-allocation-failures.md#solutions).

toocheck hello storlekar som finns tillgängliga i din region finns [Microsoft Azure: produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-toolimitsquotasconstraints-on-my-subscription-or-service"></a>Varför distribuera ett moln tjänsten tid misslyckas på grund av toolimits/kvoter/begränsningar för min prenumeration eller tjänst?
Distribution av en molnbaserad tjänst kan misslyckas om hello-resurser som är nödvändiga toobe allokerade överskrider hello standard eller maxkvoten som tillåts för tjänsten på hello region/datacenter nivå. Mer information finns i [molntjänster begränsar](../azure-subscription-service-limits.md#cloud-services-limits).

Du kan också spåra hello aktuella/kvot för din prenumeration på hello portal: Azure Portal = > prenumerationer = > \<lämpliga prenumeration > = > ”användning + kvot”.

Information om användning/förbrukning-relaterade resurser kan också hämtas via hello Azure Billing API: er. Se [Azure Resursanvändning API (förhandsgranskning)](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-hello-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Hur kan jag ändra hello storleken på en distribuerad molntjänst för virtuell dator utan att omdistribuera det?
Du kan inte ändra hello VM-storlek för en distribuerad molntjänst utan att omdistribuera den. hello VM-storlek är inbyggd i hello CSDEF som kan bara uppdateras med en ny distribution.

Mer information finns i [hur tooupdate en molntjänst](cloud-services-update-azure-service.md).

 
