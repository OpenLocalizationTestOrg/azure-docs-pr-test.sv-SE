---
title: "aaaAzure kostnaden för Advisor-rekommendationer | Microsoft Docs"
description: "Använd Azure Advisor toooptimize hello kostnaden för Azure-distributioner."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 50f70c33a17f550c8753795435cdfddd51e409f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-cost-recommendations"></a>Kostnad Advisor-rekommendationer

Advisor hjälper dig att optimera och minska din övergripande Azure tillbringar med att identifiera inaktiv och underutnyttjade resurser. Du kan hämta cost rekommendationerna från hello **kostnaden** fliken på instrumentpanelen för hello Advisor.

![Advisor kostnaden fliken](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>Optimera virtuella tillbringar genom att ändra storlek underutnyttjade instanser 
Även om vissa Programscenarier kan resultera i lågt utnyttjande av design, kan du ofta spara pengar genom att hantera hello storlek och antalet virtuella datorer. Advisor övervakar din användning av virtuella datorer på 14 dagar och identifierar låg belastning virtuella datorer. Virtuella datorer vars CPU-användning är 5 procent eller mindre och nätverksanvändning är 7 MB eller mindre för fyra eller flera dagar betraktas som låg belastning virtuella datorer.

Advisor visar du hello uppskattade kostnaden för fortsätter toorun den virtuella datorn så att du kan välja tooshut den ned eller ändra storlek på den.  

![Advisor kostnad rekommendationer för storleksändring av virtuella datorer](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-toomanage-performance-goals-of-multiple-sql-databases"></a>Använd en kostnadseffektiv lösning toomanage prestandamål av flera SQL-databaser
Advisor identifierar SQL server-instanser som kan dra nytta av att skapa elastiska databaspooler. Elastiska databaspooler ger en enkel och kostnadseffektiv lösning toomanage hello prestandamål av flera databaser som har olika användningsmönster. Läs mer om Azure elastiska pooler [vad är en Azure elastisk pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

![Advisor kostnad rekommendationer för elastiska databaspooler](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-tooaccess-cost-recommendations-in-azure-advisor"></a>Hur tooaccess kostnad rekommendationerna i Azure Advisor

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Hello vänster klickar du på **fler tjänster**.

3. I hello tjänsten menyn rutan under **övervakning och hantering av**, klickar du på **Azure Advisor**.  
 hello Advisor instrumentpanelen visas.

4. Klicka på hello Advisor instrumentpanelen hello **kostnaden** fliken.

5. Välj hello prenumeration som du vill tooreceive rekommendationer och klicka sedan på **få rekommendationer**.

> [!NOTE]
> tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor. En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen. Det här är en *engångsåtgärd*. När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.

## <a name="next-steps"></a>Nästa steg

toolearn mer om Advisor-rekommendationer finns:
* [Introduktion tooAdvisor](advisor-overview.md)
* [Kom igång](advisor-get-started.md)
* [Advisor-rekommendationer](advisor-cost-recommendations.md)
* [Advisor-rekommendationer för hög tillgänglighet](advisor-cost-recommendations.md)
* [Advisor säkerhetsrekommendationer](advisor-cost-recommendations.md)
