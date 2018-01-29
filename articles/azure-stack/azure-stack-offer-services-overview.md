---
title: "Erbjuda tjänster i Azure-stacken | Microsoft Docs"
description: "Som en moln-operatör kan du erbjuda tjänster till dina användare."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: erikje
ms.openlocfilehash: 7a54771d99f2719fcc345496b152a5d3acc02121
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Översikt över erbjuda tjänster i Azure-stacken

*Gäller för: Azure Stack integrerat system och Azure-stacken Development Kit*

[Microsoft Azure-stacken](azure-stack-poc.md) är en hybrid cloud-plattform som gör att du kan leverera tjänster från ditt datacenter. Som en leverantör, kan du erbjuda tjänster till dina klienter. Du kan erbjuda lokala tjänster till dina anställda inom ett företag eller myndighet. De tjänster som du kan leverera inkludera, men är inte begränsade till:

- Plattform som en tjänst (PaaS)-tjänster som Apptjänster, Mobilappar, API Apps, API-funktioner, SQL, MySQL.

Du kan även kombinera tjänster för att integrera och skapa komplexa lösningar för olika användare.

För att leverera tjänsterna till dina användare, måste du skapa [planer, erbjudanden och kvoter](azure-stack-plan-offer-quota-overview.md). Användarna kan sedan prenumerera på dina erbjudanden att använda tjänsterna.

## <a name="plan-your-service-offers"></a>Planera service-erbjudanden

När du planerar din erbjudanden, Tänk på följande:

**Utvärderingserbjudanden**: du kan använda utvärderingserbjudanden för att dra nya användare som kan sedan uppgradera till ytterligare tjänster till. Skapa en utvärderingsversion kan skapa en liten [Basplan](azure-stack-plan-offer-quota-overview.md#base-plan) med ett valfritt större tillägg plan.

**Kapacitetsplanering**: du kanske orolig användare ta tag stora mängder resurser och belastning system för alla användare. För att hjälpa prestanda, kan du [konfigurera planerna med kvoter](azure-stack-plan-offer-quota-overview.md#plans) cap Usage.

**Delegerad providers**: du kan ge andra användare möjlighet att skapa erbjudanden i din miljö. Om du är en tjänstprovider, du kan exempelvis [delegera](azure-stack-delegated-provider.md) denna möjlighet att din återförsäljare. Eller, om du är en organisation kan du delegera till andra avdelningar/dotterbolag.

## <a name="next-steps"></a>Nästa steg
[Mer information om erbjudanden, planer, kvoter och prenumerationer](azure-stack-plan-offer-quota-overview.md)

