---
title: "aaaOverview av Azure-databas för PostgreSQL relationsdatabstjänst | Microsoft Docs"
description: "Innehåller en översikt över Azure-databas för PostgreSQL relationsdatabstjänst."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 08/01/2017
ms.openlocfilehash: fd6821b56e5295b8b341d87b14d113f7a4b247c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Vad är Azure-databas för PostgreSQL?

Azure PostgreSQL-databas är en relationsdatabastjänst i hello Microsoft cloud inbyggda för utvecklare baserat på hello gruppversion med öppen källkod [PostgreSQL](https://www.postgresql.org/) databasmotorn. Tjänsten är tillgänglig som förhandsversion. Azure-databas för PostgreSQL ger:
- Förutsägbar prestanda på flera servicenivåer
- Dynamisk skalbarhet utan avbrott för programmet
- Inbyggd hög tillgänglighet
- Dataskydd

Alla dessa funktioner kräver nästan inga administration och alla tillhandahålls utan extra kostnad. Dessa funktioner kan du toofocus på snabb utveckling och accelerera din tid toomarket i stället allokera värdefull tid och resurser toomanaging virtuella datorer och infrastruktur. Du kan dessutom fortsätta toodevelop ditt program med hello Öppna källa verktyg och plattform du önskar och leverera med hello hastighet och effektivitet verksamheten kräver utan toolearn nya kunskaper. 

Den här artikeln är en introduktion tooAzure databas för PostgreSQL grundläggande begrepp och funktioner relaterade tooperformance, skalbarhet och hanterbarhet. Se dessa quick startar tooget som du startade:

- [Skapa en Azure-databas för PostgreSQL med hjälp av Azure portal](quickstart-create-server-database-portal.md)
- [Skapa en Azure-databas för PostgreSQL med hello Azure CLI](quickstart-create-server-database-azure-cli.md)

En uppsättning Azure CLI- och PowerShell-exempel finns här:

- [Azure CLI-exempel för Azure-databas för PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Justera prestanda och skalning utan avbrott

Azure-databas för PostgreSQL-tjänsten erbjuder två tjänstnivåer för tillfället: Basic och Standard. Varje tjänstnivå erbjuder [olika nivåer av prestanda, IOPS garantier och funktioner](concepts-service-tiers.md) toosupport lightweight tooheavyweight arbetsbelastningar. Du kan skapa din första app på en liten server för några kronor i månaden och sedan [ändra hello prestandanivå](scripts/sample-scale-server-up-or-down.md) inom tjänsten tjänstnivån manuellt eller automatiskt på alla tid toomeet hello behoven för din lösning. Du kan göra detta utan driftavbrott tooyour program eller tooyour kunder. Dynamisk skalbarhet aktiverar databasen-tootransparently svara toorapidly ändra resurskraven och aktiverar du tooonly betalar för hello resurser som du behöver när du behöver dem.

## <a name="monitoring-and-alerting"></a>Övervakning och avisering
Hur du bestämmer dig för när toodial uppåt och nedåt? Du kan använda hello inbyggda prestandaövervakning och avisering funktioner, kombinerat med hello prestanda baserat på Compute-enheter. Använda dessa verktyg kan du bedöma snabbt hello effekten av Compute enheter skala upp eller ned baserat på din aktuella eller planerade prestandabehov. Mer information finns i [Azure-databas för PostgreSQL-alternativ och prestanda: förstå vad som är tillgängliga i varje tjänstnivå](./concepts-service-tiers.md).

## <a name="keep-your-app-and-business-running"></a>Håll igång din app och din verksamhet
Azures branschledande 99,99% tillgänglighet (inte tillgängligt i förhandsvisning) servicenivåavtal (SLA) tillhandahålls av ett globalt nätverk av Microsoft-hanterade Datacenter gör att din app igång 24/7. Med varje Azure-databas för PostgreSQL-server kan du dra nytta av inbyggd säkerhet, feltolerans och dataskydd som du annars skulle ha toobuy eller design, skapa och hantera. Med Azure-databas för PostgreSQL varje tjänstnivå erbjuder ett omfattande utbud av funktionerna för verksamhetskontinuitet och alternativ som du kan använda tooget upp och körs och fortsätter att på så sätt. Du kan använda [point-in-time-återställning](howto-restore-server-portal.md) tooreturn en databas tooan tidigare tillstånd, så långt tillbaka som 35 dagar. Dessutom kan hello datacenter som är värd för dina databaser skulle få ett avbrott, du återställa databaser från geo-redundant kopior av nya säkerhetskopior.

## <a name="secure-your-data"></a>Skydda dina data
Tjänster för Azure-databas har en tradition av säkerhet för data att Azure-databas för PostgreSQL upprätthåller med funktioner som begränsar åtkomst, skydda data i vila och under rörelse och hjälper dig att övervaka aktiviteten. Besök hello [Azure Säkerhetscenter](https://www.microsoft.com/TrustCenter/Security/default.aspx) information om azures plattformssäkerhet.

hello Azure-databas för PostgreSQL-tjänsten använder kryptering för data i vila. Data, inklusive säkerhetskopior krypteras på disken (med undantag för hello av tillfälliga filer som skapas av hello-motorn när du kör frågor). hello används AES 256-bitars chiffer som ingår i Azure storage kryptering och hello nycklar är hanteras av datorn. Lagringskryptering är alltid igång och kan inte inaktiveras.

Som standard hello Azure-databas för PostgreSQL-tjänsten är konfigurerade toorequire [SSL anslutningssäkerhet](./concepts-ssl-connection-security.md) för data i rörelse över hello nätverk. Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.  Alternativt kan inaktivera du kräver SSL för att ansluta tooyour databastjänsten om klientprogrammet inte har stöd för SSL-anslutning.

## <a name="next-steps"></a>Nästa steg
- Se hello [sida med priser](https://azure.microsoft.com/pricing/details/postgresql/) för kostnadsjämförelse och Kostnadsberäknare.
- Kom igång genom att [skapa din första Azure-databas för PostgreSQL](./quickstart-create-server-database-portal.md).
- Skapa din första app i Python, PHP, Ruby, C\#, Java, Node.js: [anslutningsbibliotek](./concepts-connection-libraries.md)
