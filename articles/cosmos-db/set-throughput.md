---
title: "aaaProvision genomströmning för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur tooset etablerat dataflöde för din Azure Cosmos DB containsers, samlingar, diagram och tabeller."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: c143f4aace466b7109168a50e2eb80ddeca6400e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Ange dataflöde för Azure DB som Cosmos-behållare

Du kan ange dataflöde för din Azure DB som Cosmos-behållare i hello Azure-portalen eller med hjälp av hello klient-SDK:. 

hello i den följande tabellen listas hello genomströmning tillgänglig för behållare:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Enskild Partition behållare</strong></p></td>
            <td valign="top"><p><strong>Partitionerade behållare</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minsta dataflöde</p></td>
            <td valign="top"><p>400 frågeenheter per sekund</p></td>
            <td valign="top"><p>2 500 frågeenheter per sekund</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximalt dataflöde</p></td>
            <td valign="top"><p>10 000 frågeenheter per sekund</p></td>
            <td valign="top"><p>Obegränsat</p></td>
        </tr>
    </tbody>
</table>

## <a name="tooset-hello-throughput-by-using-hello-azure-portal"></a>tooset hello dataflöde med hjälp av hello Azure-portalen

1. Öppna i ett nytt fönster hello [Azure-portalen](https://portal.azure.com).
2. Klicka på vänster hello-fältet **Azure Cosmos DB**, eller klicka på **fler tjänster** hello längst ned i Bläddra för**databaser**, och klicka sedan på **Azure Cosmos DB**.
3. Välj Cosmos-DB-konto.
4. I hello nya fönstret, klickar du på **Data Explorer (förhandsgranskning)** i hello navigeringsmenyn.
5. Expandera din databas och behållare hello nya fönstret, och klicka sedan på **skala & inställningar**.
6. Ange hello nya genomströmning värde i hello i hello nya fönstret, **genomströmning** rutan och klicka på **spara**.

<a id="set-throughput-sdk"></a>

## <a name="tooset-hello-throughput-by-using-hello-documentdb-api-for-net"></a>tooset hello dataflöde med hello DocumentDB API för .NET

```C#
//Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput toohello new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Genomströmning vanliga frågor och svar

**Kan jag in min genomströmning tooless än 400 RU/s?**

400 RU/s är hello minsta dataflöde på Cosmos DB enskilda partitionssamlingar (2500 RU/s är hello minsta för partitionerade samlingar). Begära enheter anges i intervall om 100 RU/s men genomströmning går inte att ange too100 RU/s eller ett värde mindre än 400 RU/s. Om du letar efter ett kostnadseffektivt metoden toodevelop och testa Cosmos DB, kan du använda hello ledigt [Azure Cosmos DB emulatorn](local-emulator.md), som du kan distribuera lokalt utan kostnad. 

**Hur ställer jag in througput med hello MongoDB API?**

Det finns inga MongoDB API-tillägg tooset genomflöde. hello rekommendation är toouse hello DocumentDB API, som visas i [tooset hello dataflöde med hello DocumentDB API för .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Nästa steg

toolearn mer om etablering och pågående planeten skalning med Cosmos DB finns [partitionering och skalning med Cosmos DB](partition-data.md).
