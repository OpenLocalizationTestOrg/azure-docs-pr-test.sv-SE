---
title: "Introduktion tooAzure Cosmos DB: API för MongoDB | Microsoft Docs"
description: "Lär dig hur du kan använda Azure Cosmos DB toostore och fråga massiva mängder JSON-dokument med låg latens med hello populära OSS MongoDB APIs."
keywords: "Vad är MongoDB"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: anhoh
ms.openlocfilehash: 1eb88014cc4809332e3f5c109a44a55814194934
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-api-for-mongodb"></a>Introduktion tooAzure Cosmos DB: API för MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) är Microsofts globalt distribuerade databastjänst för flera datamodeller för verksamhetskritiska program. Azure Cosmos-DB tillhandahåller [nyckelfärdig global distributionsplatsen](distribute-data-globally.md), [elastisk skalbarhet av dataflöden och lagringsutrymmen](partition-data.md) över hela världen, en siffra millisekunders latens vid hello 99th percentilen, [fem väldefinierade konsekvensnivåer](consistency-levels.md), och garanteras hög tillgänglighet, alla säkerhetskopieras av [branschledande serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos-DB [indexerar automatiskt data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) utan att du toodeal med hantering av schemat och index. Det stöder flera modeller och dokument, nyckelvärde graf och kolumndatamodeller. 

![Azure Cosmos DB: MongoDB API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Cosmos DB databaser kan användas som hello datalager för appar som skrivits för [MongoDB](https://docs.mongodb.com/manual/introduction/). Detta innebär att med hjälp av befintliga [drivrutiner](https://docs.mongodb.org/ecosystem/drivers/), ditt program som skrivits för MongoDB nu kan kommunicera med Cosmos-DB och använda Cosmos-DB-databaser i stället för MongoDB-databaser. I många fall kan du växla från att använda MongoDB tooCosmos DB genom att ändra en anslutningssträng. Med den här funktionen kan du enkelt skapa och köra MongoDB databasprogram i hello Azure moln med Azure Cosmos DB global distributionsplatsen och [omfattande branschledande serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db), medan toouse bekant kunskaper och verktyg för MongoDB.


## <a name="what-is-hello-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Vad är hello fördelen med att använda Azure Cosmos DB för MongoDB program?

**Elastiska och skalbara dataflöden och lagring:** enkelt skala upp eller ned din MongoDB databasen toomeet programmet behöver. Dina data lagras på SSD-diskar (Solid State Disk) för korta och förutsägbara svarstider. Cosmos DB stöder MongoDB samlingar som kan skalas toovirtually obegränsade lagringsstorlekar och dataflöde. Du kan Elastiskt skala Cosmos DB med förutsägbar prestanda sömlöst när programmet växer. 

**Flera regioner replikering:** Cosmos DB replikerar transparent tooall dataområden som du har kopplat till ditt konto för MongoDB, aktivera toodevelop program som kräver global åtkomst toodata samtidigt som man kompromisser mellan din konsekvens, tillgänglighet och prestanda, alla med motsvarande garantier. Cosmos DB ger transparent regional växling vid fel med flera API: er och hello möjlighet tooelastically skalbara dataflöden och lagringsutrymmen över hello jordglob. Läs mer i [distribuera data globalt](distribute-data-globally.md).

**MongoDB kompatibilitet**: du kan använda din befintliga MongoDB kunskaper programkod och verktygsuppsättning. Du kan utveckla program som använder MongoDB och distribuera dem tooproduction med hello fullständigt hanterade globalt distribuerad Cosmos-DB-tjänst.

**Inga serverhantering**: du inte har toomanage och skala MongoDB-databaser. Cosmos DB är en helt hanterad tjänst, vilket innebär att du inte har toomanage någon infrastruktur och de virtuella datorerna själv. Cosmos DB finns i 30 + [Azure-regioner](https://azure.microsoft.com/regions/services/).

**Justerbara konsekvensnivåer:** väljer från fem väldefinierade konsekvenskontroll nivåer tooachieve optimal kompromiss mellan konsekvens och prestanda. Cosmos DB erbjuder fem olika konsekvensnivåer för frågor och läsåtgärder: stark, begränsat föråldrad, session, konsekvent prefix och eventuell. Dessa detaljerade, väldefinierade konsekvensnivåerna kan toomake själv avgöra balansen mellan konsekvens, tillgänglighet och svarstid. Läs mer i [med konsekvenskontroll nivåer toomaximize tillgänglighet och prestanda](consistency-levels.md).

**Automatisk indexering**: som standard Cosmos DB automatiskt indexerar alla hello egenskaper i dokument i MongoDB-databas och inte förväntar sig eller kräver något schema eller att sekundärindex.

**Enterprise-klass** -Azure Cosmos DB stöder flera lokala repliker toodeliver 99,99% tillgänglighet och dataskydd i hello min lokala och regionala fel. Azure Cosmos-DB har enterprise-klass [kompatibilitet certifieringar](https://www.microsoft.com/trustcenter) och säkerhetsfunktioner. 

Mer information i den här Azure fredag video med Scott Hanselman och Azure Cosmos DB Principal tekniker Manager Kirill Gavrylyuk.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-tooget-started"></a>Hur tooget igång

Följ hello MongoDB Snabbstart toocreate ett Cosmos-DB-konto och migrera dina befintliga Mongo DB programmet toouse Cosmos DB eller skapa en ny:

* [Migrera en befintlig MongoDB för Node.js-webbapp](create-mongodb-nodejs.md).
* [Skapa en webbapp i MongoDB-API med .NET och hello Azure-portalen](create-mongodb-dotnet.md)
* [Skapa en MongoDB-API-konsolapp med Java och hello Azure-portalen](create-mongodb-java.md)

## <a name="next-steps"></a>Nästa steg

Information om Azure Cosmos DB MongoDB API är integrerat i hello övergripande Azure Cosmos DB dokumentation, men här följer några tips tooget du startade:

* Följ hello [ansluta tooa MongoDB konto](connect-mongodb-account.md) självstudiekursen toolearn hur tooget informationen i anslutningssträngen ditt konto.
* Följ hello [MongoChef för användning med Azure Cosmos DB](mongodb-mongochef.md) självstudiekursen toolearn hur toocreate en anslutning mellan Azure Cosmos-DB-databas och MongoDB-app i MongoChef.
* Följ hello [migrera data tooAzure Cosmos DB med Protokollstöd för MongoDB](mongodb-migrate.md) självstudiekursen tooimport dina data tooan API för MongoDB-databas.
* Ansluta tooan API: et för MongoDB kontot med [Robomongo](mongodb-robomongo.md).
* Lär dig hur många RUs åtgärderna använder med hello [GetLastRequestStatistics kommando och hello Azure portal mått](request-units.md#GetLastRequestStatistics).
* Lär dig hur för[konfigurera Läs inställningar för globalt distribuerade appar](../cosmos-db/tutorial-global-distribution-mongodb.md).
