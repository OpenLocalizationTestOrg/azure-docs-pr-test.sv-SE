---
title: "aaaOnline säkerhetskopiering och återställning med Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur tooperform automatisk säkerhetskopiering och återställning i en Azure Cosmos-DB-databas."
keywords: "säkerhetskopiering och återställning, säkerhetskopiering online"
services: cosmos-db
documentationcenter: 
author: RahulPrasad16
manager: jhubbard
editor: monicar
ms.assetid: 98eade4a-7ef4-4667-b167-6603ecd80b79
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/11/2017
ms.author: raprasa
ms.openlocfilehash: a0b464c95681dfc7b5462b02bf04c2c43d6bc16f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Automatisk online säkerhetskopiering och återställning med Azure Cosmos DB
Azure Cosmos-DB tar automatiskt säkerhetskopior av dina data med jämna mellanrum. hello automatiska säkerhetskopieringar tas utan att påverka hello prestanda eller tillgänglighet för dina databasåtgärder. Alla säkerhetskopior lagras separat i en annan storage-tjänst och dessa säkerhetskopieringar replikeras globalt för återhämtning mot regionala katastrofer. hello automatisk säkerhetskopiering är avsedda för situationer när du av misstag tar bort Comos DB-behållaren och senare behöver återställa data eller en lösning för katastrofåterställning.  

Den här artikeln börjar med en snabb sammanfattning hello dataredundans och tillgänglighet i Cosmos-DB och beskriver säkerhetskopieringar. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Hög tillgänglighet med Cosmos-DB - en sammanfattning
Cosmos DB är utformad toobe [globalt distribuerade](distribute-data-globally.md) – möjliggör dessutom tooscale genomströmning över flera Azure-regioner tillsammans med principen som drivs redundans och transparent flera API: er. Som en databas för datalagring [99,99% tillgänglighet SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), alla hello skrivningar i Cosmos-databasen är varaktigt allokerat toolocal diskar genom att ett kvorum av replikerna i ett lokalt Datacenter innan bekräftades toohello klienten. Observera att hello hög tillgänglighet för Cosmos DB förlitar sig på lokal lagring och inte är beroende av någon extern lagringstekniker. Dessutom, om ditt konto är kopplat till mer än en Azure-region, replikeras din skrivningar samt andra regioner. tooscale genomflöde och åtkomst till data på låg latens, du kan använda så många läsa regioner som är kopplad till ditt konto som du vill. I varje skrivskyddade region sparas varaktigt hello (replikerad) data i en replik.  

Enligt beskrivningen i följande diagram hello en enskild Cosmos-DB-behållare är [vågrätt partitionerade](partition-data.md). En ”partition” markeras med en cirkel i följande diagram hello och varje partition görs högtillgänglig via en replikuppsättning. Detta är hello lokala distributionsplatser i ett enda Azure-region (betecknas med hello X-axeln). Dessutom kan distribueras varje partition (med dess motsvarande replikuppsättningen) sedan globalt över flera regioner som är kopplad till ditt konto (till exempel i den här bilden hello tre regioner – östra USA, västra USA och centrala Indien). Hej ”partition ange” är ett globalt distribuerade entitet som består av flera kopior av dina data i varje region (betecknas med hello Y-axeln). Du kan tilldela prioritet toohello regioner som är kopplad till ditt konto och Cosmos DB kommer transparent redundans toohello nästa område händelse av katastrof. Du kan också manuellt simulera redundans tootest hello slutpunkt till slutpunkt tillgängligheten för ditt program.  

hello illustrerar följande bild hello hög grad av redundans med Cosmos DB.

![Hög grad av redundans med Cosmos DB](./media/online-backup-and-restore/redundancy.png)

![Hög grad av redundans med Cosmos DB](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Fullständig, automatisk, online-säkerhetskopieringar
OJ, bort jag min behållare eller databasen! Med Cosmos DB görs också mycket överflödiga och flexibel tooregional katastrofer i inte bara din data, men hello säkerhetskopior av dina data. Dessa säkerhetskopior av automatiserad tas för närvarande cirka var fjärde timme och lagras senaste 2 säkerhetskopior vid alla tidpunkter. Om hello data av misstag tas bort eller är skadad, kontrollera [kontakta Azure-supporten](https://azure.microsoft.com/support/options/) inom 8 timmar. 

hello säkerhetskopieringar tas utan att påverka hello prestanda eller tillgänglighet för dina databasåtgärder. Cosmos DB tar hello säkerhetskopiering i hello bakgrunden utan att använda din etablerade RUs eller påverka hello prestanda och utan att påverka hello tillgängligheten för din databas. 

Till skillnad från dina data som lagras i Cosmos DB lagras hello automatiska säkerhetskopior i Azure Blob Storage-tjänst. tooguarantee hello låg latens/effektiv överföring hello ögonblicksbilden av säkerhetskopian är överförda tooan instans av Azure Blob storage i samma region som hello aktuella skrivåtgärder region där din Cosmos-DB-databaskonto hello. För återhämtning mot regionala replikeras igen varje ögonblicksbild av dina säkerhetskopierade data i Azure Blob Storage via geo-redundant lagring (GRS) tooanother region. hello följande diagram visar att hello hela Cosmos-DB-behållaren (med alla tre primära partitioner i västra USA i det här exemplet) säkerhetskopieras i en fjärransluten Azure Blob Storage-konto i USA, västra och sedan GRS replikeras tooEast oss. 

hello illustrerar följande bild periodiska fullständiga säkerhetskopieringar för alla Cosmos-DB-enheter i GRS Azure Storage.

![Periodiska fullständiga säkerhetskopieringar för alla Cosmos-DB-enheter i Azure Storage för GRS](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Period för lagring av säkerhetskopior.
Enligt beskrivningen ovan Azure Cosmos DB tar ögonblicksbilder av data var fjärde timme och behåller hello två sista ögonblicksbilder av varje partition i 30 dagar. Per våra förordningar rensas ögonblicksbilder efter 90 dagar.

Om du vill toomaintain egna ögonblicksbilder, du kan använda hello exportalternativ tooJSON i hello Azure Cosmos DB [datamigreringsverktyget](import-data.md#export-to-json-file) tooschedule ytterligare säkerhetskopieringar. 

## <a name="restoring-a-database-from-an-online-backup"></a>Återställa en databas från en onlinesäkerhetskopiering
Om du av misstag tar bort den databas eller en samling kan du [filen ett supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) eller [kontakta Azure support](https://azure.microsoft.com/support/options/) toorestore hello data från hello senaste automatisk säkerhetskopiering. Om du behöver toorestore databasen på grund av att data skadas problemet, se [hantering datafel](#handling-data-corruption) som du behöver tootake ytterligare steg tooprevent hello skadade data från inträngande hello säkerhetskopior. För en specifik ögonblicksbild av din backup toobe återställts kräver Cosmos DB hello data var tillgänglig för hello varaktighet hello säkerhetskopiering för den ögonblicksbilden.

## <a name="handling-data-corruption"></a>Hantering av skadade data
Azure Cosmos-DB behåller hello senaste två säkerhetskopieringar för varje partition i hello system. Den här modellen fungerar mycket bra när en behållare (insamling av dokument, diagram, tabell) eller en databas oavsiktligt tas bort eftersom en av hello senaste versioner kan återställas. Dock i hello kan om när ett problem med skadade data kan innebära att användare, Azure Cosmos DB kan vara ovetande om hello datafel och det är möjligt att hello skadas ha genombryts hello säkerhetskopieringar. Så snart skadad är, bör du ta bort hello skadad behållare (tabell-samling/diagram) så att säkerhetskopieringar skyddas från att skrivas över med skadade data. Eftersom hello senaste säkerhetskopiering kan vara fyra timmar gamla hello användare kan använda [ändra feed](change-feed.md) toocapture och lagra hello senaste fyra timmar värt data innan du tar bort hello behållare.

## <a name="next-steps"></a>Nästa steg

tooreplicate databasen i flera datacenter, se [distribuera dina data globalt med Cosmos DB](distribute-data-globally.md). 

toofile kontakta Azure-Support, [filen en biljett från hello Azure-portalen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

