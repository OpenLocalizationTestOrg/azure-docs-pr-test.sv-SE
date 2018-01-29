---
title: "Hög tillgänglighet koncept i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Det här avsnittet innehåller information för hög tillgänglighet när du använder Azure-databas för PostgreSQL"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 600896cf064770a5b294f874dc29081f0ce7d942
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/03/2017
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>Hög tillgänglighet koncept i Azure-databas för PostgreSQL
Azure-databasen för PostgreSQL-tjänsten tillhandahåller garanterad hög tillgänglighet. Ekonomiskt säkerhetskopieras servicenivåavtalet (SLA) är 99,99% vid allmän tillgänglighet. Det är praktiskt taget inget program stillestånd när du använder den här tjänsten.

## <a name="high-availability"></a>Hög tillgänglighet
Hög tillgänglighet (HA)-modellen baseras på inbyggda failover-funktioner när en nod på objektnivå avbrott inträffar. Ett avbrott nivån kan inträffa på grund av ett maskinvarufel eller som svar på en tjänstdistribution.

Vid alla tidpunkter uppstå ändringar som gjorts i en Azure-databas för PostgreSQL databasserver i samband med en transaktion. Ändringar registreras i Azure storage synkront när genomförs transaktionen. Om en nod på objektnivå avbrott inträffar databasservern automatiskt skapar en ny nod och lagring av data till den nya noden. Alla aktiva anslutningar tas bort och alla inflight transaktioner genomförs inte.

## <a name="application-retry-logic-is-essential"></a>Logik för omprövning av programmet krävs
Det är viktigt att PostgreSQL databasprogram har skapats för att identifiera och försök avbrutna anslutningar och inte kunde transaktioner. När programmet försöker omdirigeras transparent programmets anslutning till den nyligen skapade instansen, som tar för instansen som misslyckats.

Internt i Azure används en gateway för att dirigera om anslutningar till den nya instansen. Vid ett avbrott ta failover hela processen brukar flera sekunder. Eftersom omdirigeringen hanteras internt av gateway, densamma externa anslutningssträngen för klientprogrammen.

## <a name="scaling-up-or-down"></a>Skala upp eller ned
Liknar modellen hög tillgänglighet, när en Azure-databas för PostgreSQL skalas upp eller ned, en ny serverinstans med den angivna storleken skapas. Befintliga datalagring är frånkopplat från den ursprungliga instansen och anslutna till den nya instansen.

Under åtgärden sker ett avbrott databasanslutningarna. Klientprogrammen är frånkopplad och öppna ogenomförda transaktioner har avbrutits. När klientprogrammet försöker upprätta anslutningen, eller gör en ny anslutning, dirigerar gatewayen anslutningen till nyligen storlek instansen. 

## <a name="next-steps"></a>Nästa steg
- En översikt över tjänsten finns [Azure Database PostgreSQL-översikt](overview.md)
