---
title: aaaAzure Advisor-rekommendationer | Microsoft Docs
description: "Använda Advisor toooptimize hello prestandan för din Azure-distributioner."
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
ms.openlocfilehash: eb3d928664717f6f322132ac740f42015f56b76e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-performance-recommendations"></a>Advisor-rekommendationer

Azure Advisor-rekommendationer förbättra hello hastighet och tillgänglighet för verksamhetskritiska program. Du kan få rekommendationer från Advisor på hello **prestanda** fliken hello Advisor instrumentpanelen.

![Fliken för Advisor-prestanda](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a>Förbättra prestanda för databasen med SQL DB Advisor

Advisor ger en konsekvent konsoliderad vy över rekommendationer för alla dina Azure-resurser. Den kan integreras med SQL Database Advisor toobring du rekommendationer för att förbättra hello prestandan för din SQL Azure-databas. SQL Database Advisor utvärderar hello prestanda för SQL Azure-databaser genom att analysera din användningshistorik. Den erbjuder sedan rekommendationer som passar bäst för att köra hello databasen vanlig arbetsbelastning. 

> [!NOTE]
> tooget rekommendationer som en databas måste ha om en vecka för användning och som veckan måste det finnas vissa konsekvent aktivitet. SQL Database Advisor kan optimera enklare för konsekvent frågemönster än för slumpmässiga belastning för aktiviteten.

Mer information om SQL Database Advisor finns [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![Rekommendationer för SQL-databas](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a>Förbättra Redis-Cache-prestanda och tillförlitlighet

Advisor identifierar Redis-Cache instanser där prestanda kan påverkas negativt av hög minnesanvändning, serverbelastning, bandbredd i nätverket eller ett stort antal klientanslutningar. Advisor innehåller även best practices rekommendationer toohelp du undvika potentiella problem. Mer information om Redis-Cache rekommendationer finns [Redis-Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Förbättra Apptjänst prestanda och tillförlitlighet

Azure Advisor integrerar rekommendationer om bästa praxis för att förbättra din upplevelse Apptjänster och identifiera relevanta plattformsfunktioner. Exempel på Apptjänster rekommendationer är:
* Identifiera instanser där minne eller CPU-resurserna är uttömda av app körningar med alternativ för lösning.
* Identifiera instanser där collocating resurser som webbappar och databaser kan förbättra prestanda och den lägre kostnaden. 

Mer information om Apptjänster rekommendationer finns [bästa praxis för Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![Rekommendationer för App-tjänster](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-tooaccess-performance-recommendations-in-advisor"></a>Hur tooaccess rekommendationer i Advisor

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Hello vänster klickar du på **fler tjänster**.

3. I hello tjänsten menyn rutan under **övervakning och hantering av**, klickar du på **Azure Advisor**.  
 hello Advisor instrumentpanelen visas.

4. Klicka på hello Advisor instrumentpanelen hello **prestanda** fliken.

5. Välj hello prenumeration som du vill tooreceive rekommendationer och klicka sedan på **få rekommendationer**.

> [!NOTE]
> tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor. En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen. Det här är en *engångsåtgärd*. När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.

## <a name="next-steps"></a>Nästa steg

toolearn mer om Advisor-rekommendationer finns:

* [Introduktion tooAdvisor](advisor-overview.md)
* [Kom igång med Advisor](advisor-get-started.md)
* [Kostnad Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Advisor-rekommendationer för hög tillgänglighet](advisor-high-availability-recommendations.md)
* [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md)

