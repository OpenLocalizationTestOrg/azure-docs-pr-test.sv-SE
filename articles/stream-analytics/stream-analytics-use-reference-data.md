---
title: "Använd data och slå upp referenstabellerna i Stream Analytics | Microsoft Docs"
description: "Använda referensdata i en Stream Analytics-fråga"
keywords: uppslagstabellen referensdata
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 438ec565f3c6e06ab7ec92cf1bbfbdde88f99b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Med hjälp av data- eller lookup register i en Stream Analytics Indataströmmen
Referensdata (även kallat en uppslagstabell) är en begränsad mängd data som är statisk eller saktas ändra karaktär används för att utföra en sökning eller att korrelera med din dataström. För att använda referensdata i Azure Stream Analytics-jobbet, använder du normalt en [referens Data ansluta](https://msdn.microsoft.com/library/azure/dn949258.aspx) i frågan. Stream Analytics använder Azure Blob storage som lagringsskikt för referensdata med Azure Data Factory-referensen kan data omvandlas eller kopieras till Azure Blob storage för användning som referensdata från [valfritt antal molnbaserade och lokalt datalager](../data-factory/copy-activity-overview.md). Referensdata modelleras som en serie blobbar (definieras i den inkommande configuration) i stigande ordning efter datum/tid som anges i blob-namn. Den **endast** har stöd för att lägga till i slutet av sekvensen med hjälp av ett datum/tid **större** än den som angetts av den senaste blobben i sekvensen.

Stream Analytics har en **gränsen på 100 MB per blob** men jobb kan bearbeta flera referens blobbar med hjälp av den **sökvägar** egenskapen.


## <a name="configuring-reference-data"></a>Konfigurera referensdata
Om du vill konfigurera din referensdata måste du först skapa indata som är av typen **referensdata**. Tabellen nedan beskriver varje egenskap måste du ange när du skapar referens indata med beskrivningen:


<table>
<tbody>
<tr>
<td>Egenskapsnamn</td>
<td>Beskrivning</td>
</tr>
<tr>
<td>Ett inmatat Alias</td>
<td>Ett eget namn som ska användas i jobb frågan för att referera till den här indata.</td>
</tr>
<tr>
<td>Lagringskonto</td>
<td>Namnet på det lagringskonto där dina blobbar finns. Om den är i samma prenumeration som Stream Analytics-jobbet måste välja du den från listrutan.</td>
</tr>
<tr>
<td>Lagringskontonyckel</td>
<td>Den hemliga nyckeln som associeras med lagringskontot. Detta hämtar fylls automatiskt i om lagringskontot tillhör samma prenumeration som Stream Analytics-jobbet.</td>
</tr>
<tr>
<td>Lagringsbehållaren</td>
<td>Behållare innehåller en logisk gruppering för blobbar som lagras i Microsoft Azure Blob-tjänsten. När du överför en blobb till Blob-tjänsten måste du ange en behållare för blobben.</td>
</tr>
<tr>
<td>Sökvägar</td>
<td>Sökvägen som används för att hitta dina blobbar i den angivna behållaren. I sökvägen, kan du välja att ange en eller flera instanser av följande 2 variabler:<BR>{date} {time}<BR>Exempel 1: products/{date}/{time}/product-list.csv<BR>Exempel 2: products/{date}/product-list.csv
</tr>
<tr>
<td>[Valfritt] datumformat</td>
<td>Om du har använt {date} i sökvägar som du angav, kan du välja datumformat där dina blobbar ordnas listrutan av de format som stöds.<BR>Exempel: ÅÅÅÅ-MM/DD, åååå-MM-DD osv.</td>
</tr>
<tr>
<td>[Valfritt] tidsformat</td>
<td>Om du har använt {time} i sökvägar som du angav, kan du välja tidsformatet där dina blobbar ordnas listrutan av de format som stöds.<BR>Exempel: HH, HH/mm eller HH-mm</td>
</tr>
<tr>
<td>Händelsen serialiseringsformat</td>
<td>Om du vill kontrollera att dina frågor fungerar som förväntat, Stream Analytics behöver veta vilka serialiseringsformat använder för inkommande dataströmmar. För referens är format som stöds csv- och JSON.</td>
</tr>
<tr>
<td>Encoding</td>
<td>UTF-8 är endast stöds Kodningsformatet just nu</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generera referensdata enligt ett schema
Om din referensdata är en långsamt föränderliga datauppsättning, sedan stöd för att uppdatera referens data aktiveras genom att ange en sökväg mönster i inkommande konfigurationen med {date} och {time} ersättning token. Stream Analytics hämtar uppdaterade referensdata datadefinitioner baserat på den här sökvägen. Till exempel ett mönster för `sample/{date}/{time}/products.csv` med datumformatet **”åååå-MM-DD”** och ett tidsformat för **”HH-mm”** instruerar Stream Analytics för att hämta uppdaterade blob `sample/2015-04-16/17-30/products.csv` kl: 30 på 16 April , 2015 UTC-tidszonen.

> [!NOTE]
> För närvarande ser Stream Analytics-jobb ut för blob-uppdatering endast när datortiden avancerar till den tid som kodats i blob-namnet. Till exempel jobbet söker efter `sample/2015-04-16/17-30/products.csv` så snart som möjligt, men ingen tidigare än 5:30 PM 16 April 2015 UTC tid zonen. Kommer det att *aldrig* leta efter en blob med ett kodat tidigare än den sista som har identifierats.
> 
> T.ex. när jobbet hittar blob `sample/2015-04-16/17-30/products.csv` kommer att ignorera alla filer som har ett kodat infaller tidigare än 5:30 PM 16 April 2015 så om en sen anländer `sample/2015-04-16/17-25/products.csv` blob skapas i samma behållare jobbet kommer inte att användas.
> 
> Likaså om `sample/2015-04-16/17-30/products.csv` skapas endast klockan 10:03 16 April 2015 men ingen blob med ett tidigare datum finns i behållaren, jobbet kommer att använda den här filen från klockan 10:03 16 April 2015 och använda tidigare referensdata fram till dess.
> 
> Ett undantag startas när jobbet måste nytt bearbeta data i tid eller när jobbet. Vid start söker jobbet efter de senaste blob innan jobbet starttid har angetts. Detta görs för att kontrollera att det finns en **icke-tom** refererar till datauppsättningen när jobbet startas. Om något inte går att hitta jobbet visar följande diagnostik: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kan användas för att dirigera aktiviteten för att skapa den uppdaterade BLOB-objekt som krävs av Stream Analytics för att uppdatera referens datadefinitioner. Data Factory är en molnbaserad dataintegreringstjänst som samordnar och automatiserar förflyttning och transformering av data. Data Factory stöder [ansluter till ett stort antal moln baserade och lokala datalager](../data-factory/copy-activity-overview.md) och enkelt flytta data enligt ett schema som du anger. Mer information och steg-för-steg-anvisningar om hur du ställer in Data Factory-pipelinen för att generera referensdata för Stream Analytics som uppdateras enligt ett fördefinierat schema kolla detta [GitHub exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tips om att uppdatera referensdata
1. Skriver över referens data-blobs inte ska orsaka Stream Analytics att läsa in blob och i vissa fall kan det orsaka jobb misslyckas. Det rekommenderade sättet att ändra referensdata är att lägga till en ny blob med samma behållare och sökvägen mönstret som definieras i jobbet indata och använda en datum/tid **större** än den som angetts av den senaste blobben i sekvensen.
2. Referens för data-blobs är **inte** sorteras i blob ”senaste” modifierades men endast av datum och tid som anges i blob med {date} och {time} ersättningar.
3. På några tillfällen ett jobb måste gå tillbaka i tiden, därför referens data-blobs måste inte kan ändras eller tas bort.

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
Du har fått en introduktion till Stream Analytics, en hanteringstjänst för analys av dataströmmar från Internet of Things. Mer information om den här tjänsten finns här:

* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
