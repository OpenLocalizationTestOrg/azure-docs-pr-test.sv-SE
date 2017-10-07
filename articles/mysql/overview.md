---
title: "aaaOverview av Azure-databas för MySQL relationsdatabstjänst | Microsoft Docs"
description: "Översikt över hello Azure-databas för MySQL relationsdatabstjänst."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 08/02/2017
ms.custom: mvc
ms.openlocfilehash: f02493e2c3c38ccab408a718f98861600481812d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-database-for-mysql-service-introduction"></a>Vad är Azure-databas för MySQL? Tjänsten introduktion
Azure MySQL-databas är en relationsdatabastjänst i hello Microsoft cloud baserat på [MySQL Community Edition](https://www.mysql.com/products/community/) databasmotorn.  Det ger Azure MySQL-databas:

- Förutsägbar prestanda på flera servicenivåer
- Dynamisk skalbarhet utan avbrott för programmet
- Inbyggd hög tillgänglighet
- Dataskydd

Dessa funktioner kräver nästan inga administration och alla tillhandahålls utan extra kostnad. Att toofocus på snabb apputveckling och accelerera din tid toomarket i stället allokera värdefull tid och resurser toomanaging virtuella datorer och infrastruktur. Du kan dessutom fortsätta toodevelop ditt program med hello Öppna källa verktyg och plattform du önskar och leverera med hello hastighet och effektivitet verksamheten kräver utan toolearn nya kunskaper.

![Azure-databas för MySQL konceptuellt diagram](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Den här artikeln är en introduktion tooAzure databas för MySQL grundläggande begrepp och funktioner relaterade tooperformance, skalbarhet och hanterbarhet, med länkar tooexplore information. Se dessa quick startar tooget som du startade:
- [Skapa en Azure Database för MySQL med Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

För en uppsättning exempel som Azure CLI, se:
- [Azure CLI-exempel för Azure-databas för MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Justera prestanda och skalning utan avbrott
Azure-databas för MySQL-tjänst som erbjuder två tjänstnivåer: Basic och Standard. Varje nivå har olika prestanda och funktioner toosupport lightweight tooheavyweight arbetsbelastningar. Du kan skapa din första app på en liten databas för några kronor månad och sedan ändra ditt service tier tooscale med behov av din lösning utan avbrott. Dynamisk skalbarhet gör det möjligt för din databas tootransparently svara toorapidly behoven för resursen. Du betalar bara för hello resurser du behöver, när det behövs.

## <a name="monitoring-and-alerting"></a>Övervakning och avisering
Hur vet du hello rätt värden när man reglerar uppåt och nedåt? Använd hello inbyggda prestandaövervakning och avisering funktioner, kombinerat med hello prestanda baserat på Compute-enhet. Använder dessa funktioner kan du snabbt utvärdera hello effekten av att skala upp eller ned baserat på din aktuella eller projektet prestandabehov. Se [begrepp: tjänstnivåer](concepts-service-tiers.md) mer information.

## <a name="keep-your-app-and-business-running"></a>Håll igång din app och din verksamhet
Azures branschledande 99,99% tillgänglighet servicenivåavtal (SLA) tillhandahålls av ett globalt nätverk av Microsoft-hanterade Datacenter gör att din app igång 24/7. Med varje Azure-databas för MySQL-server kan du dra nytta av inbyggd säkerhet, feltolerans och dataskydd som du annars skulle ha toobuy eller design, skapa och hantera. Du kan använda point-in-time-återställning toorecover tooan en server med Azure-databas för MySQL tidigare tillstånd, så långt tillbaka som 35 dagar.

## <a name="secure-your-data"></a>Skydda dina data
Tjänster för Azure-databas har en tradition av säkerhet för data att Azure-databas för MySQL upprätthåller med funktioner som begränsar åtkomst, skydda data i vila och under rörelse och hjälper dig att övervaka aktiviteten. Besök hello [Azure Säkerhetscenter](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) information om azures plattformssäkerhet.

hello Azure-databas för MySQL-tjänst som använder kryptering för data i vila. Data inklusive säkerhetskopior, krypteras på disken (med undantag för hello av tillfälliga filer som skapas av hello-motorn när du kör frågor). hello används AES 256-bitars chiffer som ingår i Azure storage kryptering och hello nycklar är hanteras av datorn. Lagringskryptering är alltid igång och kan inte inaktiveras.

Som standard hello Azure-databas för MySQL-tjänst som är konfigurerade toorequire [SSL anslutningssäkerhet](./concepts-ssl-connection-security.md) för data i rörelse över hello nätverk. Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.  Alternativt kan inaktivera du kräver SSL för att ansluta tooyour databastjänsten om klientprogrammet inte har stöd för SSL-anslutning.

## <a name="next-steps"></a>Nästa steg
Nu när du har läst en introduktion tooAzure databas för MySQL och besvaras hello frågan ”vad är Azure MySQL-databas”?, är du redo att:
- Se priser för kostnadsjämförelse och Kostnadsberäknare hello. [Prissättning](https://azure.microsoft.com/pricing/details/mysql/)
- Kom igång genom att skapa den första servern. [Skapa en Azure Database för MySQL med Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Skapa din första app i Python, PHP, Ruby, C\#, Java, Node.js: [anslutning bibliotek används tooconnect tooAzure databas för MySQL](concepts-connection-libraries.md)
