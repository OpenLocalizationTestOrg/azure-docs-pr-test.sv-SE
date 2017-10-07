---
title: "aaaDocumentDB API prestandanivåer | Microsoft Docs"
description: "Läs mer om hur prestandanivåer för DocumentDB API aktivera tooreserve genomströmning på grundval av per behållare."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 716bc11ae238dbb0feebf004ed8d5f8a7515ec6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retiring-hello-s1-s2-and-s3-performance-levels"></a>Ta bort hello S1, S2 och S3 prestandanivåer

> [!IMPORTANT] 
> hello S1, S2 och S3 prestandanivåer i den här artikeln är som har återkallats och är inte längre tillgängliga för nya DocumentDB API-konton.
>

Den här artikeln innehåller en översikt över S1, S2 och S3 prestandanivåer och beskriver hur hello samlingar som använder dessa prestandanivåer kommer att migrerade toosingle partitionssamlingar på 1 augusti 2017. När du har läst den här artikeln att du kunna tooanswer hello följande frågor:

- [Varför är hello S1, S2 och S3 prestanda nivåer som har återkallats?](#why-retired)
- [Hur enskilda partitionssamlingar och partitionerade samlingar jämföra toohello S1, S2, prestandanivåer S3?](#compare)
- [Vad gör jag behöver toodo tooensure oavbrutet dataåtkomst toomy?](#uninterrupted-access)
- [Hur ändrar min samlingen efter hello migreringen?](#collection-change)
- [Hur min fakturering ändras när jag migrerade toosingle partitionssamlingar?](#billing-change)
- [Vad händer om jag behöver fler än 10 GB lagringsutrymme?](#more-storage-needed)
- [Kan jag ändra mellan hello S1, S2 och S3 prestandanivåer före den 1 augusti 2017?](#change-before)
- [Hur vet jag om min samling har migrerats?](#when-migrated)
- [Hur migrera från hello S1, S2 partitionssamlingar för S3 prestanda nivåer toosingle själv?](#migrate-diy)
- [Hur kan jag påverkas om jag en EA-kund?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-hello-s1-s2-and-s3-performance-levels-being-retired"></a>Varför är hello S1, S2 och S3 prestanda nivåer som har återkallats?

hello S1, S2 och S3 prestandanivåer gör inte ger hello flexibilitet som erbjuder DocumentDB API samlingar. Med hello S1, S2 S3 prestandanivåer båda hello genomflöde och lagringskapaciteten har redan angetts och erbjöd inte elasticitet. Azure Cosmos-DB erbjuder hello möjlighet toocustomize din dataflöden och lagringsutrymmen, erbjuder mycket mer flexibilitet i din möjlighet tooscale eftersom dina behov förändras.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-toohello-s1-s2-s3-performance-levels"></a>Hur enskilda partitionssamlingar och partitionerade samlingar jämföra toohello S1, S2, prestandanivåer S3?

hello följande tabell jämförs hello dataflöden och lagringsutrymmen alternativen i enskilda partitionssamlingar partitionerade samlingar och S1, S2 S3 prestandanivåer. Här är ett exempel på oss östra 2 region:

|   |Partitionerad samling|Enskild partition samling|S1|S2|S3|
|---|---|---|---|---|---|
|Maximalt dataflöde|Obegränsat|10 K RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Minsta dataflöde|2.5 K RU/s|400 RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Maximalt lagringsutrymme|Obegränsat|10 GB|10 GB|10 GB|10 GB|
|Pris (månad)|Genomströmning: $6 / 100 RU/s<br><br>Lagring: $ 0,25/GB|Genomströmning: $6 / 100 RU/s<br><br>Lagring: $ 0,25/GB|$25 USD|$50 USD|$100 USD|

Är du en kund med EA? I så fall, finns [hur får jag påverkas om jag en EA-kund?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-toodo-tooensure-uninterrupted-access-toomy-data"></a>Vad gör jag behöver toodo tooensure oavbrutet dataåtkomst toomy?

Något, Cosmos DB hanterar hello-migreringen. Om du har en samling S1, S2 och S3, blir din samling samling för migrerade tooa enskild partition på den 31 juli 2017. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-hello-migration"></a>Hur ändrar min samlingen efter hello migreringen?

Om du har en samling S1 kommer du att migrerade tooa enda partition samling med 400 RU/s genomströmning. 400 RU/s är hello lägsta genomströmning tillgänglig med enskilda partitionssamlingar. Dock hello hello kostnaden för 400 RU/s i en enda partition samling är ungefär hello samma som du betalar med S1 samlingen hello och 250 RU/s – så att du inte betalar för extra 150 RU/s tillgängliga tooyou.

Om du har en S2 samling, kommer du att migrerade tooa enda partition samling med 1 K RU/s. Ingen ändring tooyour genomströmning nivå visas.

Om du har en samling S3, kommer du att migrerade tooa enda partition samling med 2,5 K RU/s. Ingen ändring tooyour genomströmning nivå visas.

I dessa fall migrerat samlingen ska du kan toocustomize genomströmning-nivå, eller skala den uppåt och nedåt som behövs tooprovide låg latens åtkomst tooyour användare. toochange hello genomströmning nivå när din samling har migrerats helt enkelt öppna Cosmos-DB-konto i hello Azure-portalen, klickar du på skala, Välj din samling och sedan justera hello genomströmning nivå, som visas i följande skärmbild hello:

![Hur tooscale genomflöde i hello Azure-portalen](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-toohello-single-partition-collections"></a>Hur min fakturering ändras när jag migrerade toohello enskilda partitionssamlingar?

Om du har 10 S1 samlingar, 1 GB lagringsutrymme i hello oss Öst region, och du migrerar de här 10 S1 samlingar too10 enskilda partitionssamlingar på 400 RU/s (hello lägsta nivå). Fakturan ska se ut så här om du behåller hello 10 enskilda partitionssamlingar för en hel månad:

![Hur S1 priser för 10 samlingar jämför too10 samlingar med priser för en enskild partition samling](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Vad händer om jag behöver fler än 10 GB lagringsutrymme?

Om du har en samling med en S1, S2 och S3 prestandanivå eller ha en enda partition samling, 10 GB tillgängligt lagringsutrymme som alla har kan du använda hello Cosmos DB datamigrering verktyget toomigrate data-tooa partitionerad samling med princip obegränsad lagring. Information om hello fördelar för en partitionerad samling finns [partitionering och skalning i Azure Cosmos DB](documentdb-partition-data.md). Information om hur toomigrate din S1 S2, S3 eller enskild partition samling tooa partitionerad samling, se [migrerar från en enskild partition toopartitioned samlingar](documentdb-partition-data.md#migrating-from-single-partition). 

<a name="change-before"></a>

## <a name="can-i-change-between-hello-s1-s2-and-s3-performance-levels-before-august-1-2017"></a>Kan jag ändra mellan hello S1, S2 och S3 prestandanivåer före den 1 augusti 2017?

Endast befintliga konton med S1, S2 och S3 prestanda ska kunna toochange och ändra nivån prestandanivåer hello-portalen eller programmässigt. Med den 1 augusti 2017 hello S1 S2, och S3 prestandanivåer kommer inte längre tillgänglig. Om du ändrar från S1, S3 eller S3 tooa enda partition samling kan återgå du inte toohello S1, S2 och S3 prestandanivåer.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Hur vet jag om min samling har migrerats?

hello migreringen sker på 31 juli 2017. Om du har en samling som använder hello S1, S2 eller S3 prestandanivåer, hello Cosmos-DB-teamet kommer att kontakta dig via e-post innan hello migrering äger rum. När hello migreringen är klar på 1 augusti 2017 visar hello Azure-portalen att din samling använder standardprisnivån.

![Hur tooconfirm din samling har migrerats toohello Standard prisnivå](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-hello-s1-s2-s3-performance-levels-toosingle-partition-collections-on-my-own"></a>Hur migrera från hello S1, S2 partitionssamlingar för S3 prestanda nivåer toosingle själv?

Du kan migrera från hello S1, S2, och S3 prestanda nivåer toosingle partitionssamlingar med hello Azure-portalen eller programmässigt. Du kan göra detta på din egen innan 1 augusti toobenefit från hello flexibla genomströmning alternativen med enskilda partitionssamlingar eller kommer vi att migrera samlingar för dig på den 31 juli 2017.

**toomigrate toosingle partitionssamlingar med hello Azure-portalen**

1. I hello [ **Azure-portalen**](https://portal.azure.com), klickar du på **Azure Cosmos DB**, Välj hello Cosmos DB konto toomodify. 
 
    Om **Azure Cosmos DB** är inte på hello Jumpbar klickar du på >, rulla för**databaser**väljer **Azure Cosmos DB**, och välj sedan hello DocumentDB-konto.  

2. På menyn för hello-resurs under **behållare**, klickar du på **skala**, Välj hello samling toomodify hello nedrullningsbara listan och klicka sedan på **prisnivån**. Konton med hjälp av fördefinierade genomströmning har en prisnivå för S1, S2 och S3.  I hello **Välj din prisnivå** bladet klickar du på **Standard** toochange toouser definierats dataflöde, och klicka sedan på **Välj** toosave ändringen.

    ![Skärmbild som visar hello inställningsbladet visar där toochange hello genomströmning värde](./media/performance-levels/change-performance-set-thoughput.png)

3. Tillbaka i hello **skala** bladet, hello **prisnivån** ändras för**Standard** och hello **genomströmning (RU/s)** visas med en Standardvärdet för 400. Ange hello dataflödet mellan 400 och 10 000 [programbegäran](request-units.md)/second (RU/s). Hej **uppskattad Månadsfaktura** längst hello hello sidan uppdateras automatiskt tooprovide en uppskattning av hello kostnad varje månad. 

    >[!IMPORTANT] 
    > När du sparar ändringarna och flytta toohello Standard prisnivå återställa du inte toohello S1, S2 och S3 prestandanivåer.

4. Klicka på **spara** toosave ändringarna.

    Om du anser att du behöver mer genomströmning (större än 10 000 RU/s) eller mer lagringsutrymme (större än 10GB) kan du skapa en partitionerad samling. toomigrate en enskild partition samling tooa partitionerad samling finns [migrerar från en enskild partition toopartitioned samlingar](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Om du ändrar från S1, S2 och S3 tooStandard kan ta too2 minuter.
    > 
    > 

**toomigrate toosingle partitionssamlingar med hello .NET SDK**

Ett annat alternativ för att ändra din samlingar prestandanivåer är via vårt SDK: er. Det här avsnittet beskriver bara ändra prestanda för en samling nivå med hjälp av vår [DocumentDB .NET API](documentdb-sdk-dotnet.md), men hello processen är liknande för våra andra SDK: er.

Här är ett kodfragment för att ändra hello samling genomströmning too5 000 frågeenheter per sekund:
    
```C#
    //Fetch hello resource toobe updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set hello throughput too5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes toohello database by replacing hello original resource
    await client.ReplaceOfferAsync(offer);
```

Besök [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview ytterligare exempel och lär dig mer om våra erbjudanden metoder:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Hur kan jag påverkas om jag en EA-kund?

EA-kunder kommer att pris skyddade fram hello slutet av avtalets aktuella.

## <a name="next-steps"></a>Nästa steg
toolearn mer information om priser och hantera data med Azure Cosmos DB utforska gärna dessa resurser:

1.  [Partitionering data i Cosmos DB](documentdb-partition-data.md). Förstå hello skillnaden mellan enskild partition behållare och partitionerade behållare samt tips om hur du implementerar en partitionering strategi tooscale sömlöst.
2.  [Priser för cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). Läs mer om hello kostnaden för etablering genomflöde och använda lagring.
3.  [Enheter för programbegäran](request-units.md). Förstå hello förbrukningen av genomströmning för olika åtgärdstyper, till exempel läsa, skriva frågan.
