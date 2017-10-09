---
title: "aaaUse data och slå upp referenstabellerna i Stream Analytics | Microsoft Docs"
description: "Använda referensdata i en Stream Analytics-fråga"
keywords: uppslagstabellen referensdata
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: fb1d18fba920db5e097d0c95d333e8e8390d1589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Med hjälp av data- eller lookup register i en Stream Analytics Indataströmmen
Referensdata (även kallat en uppslagstabell) har en begränsad mängd data som är statisk eller saktas ändra karaktär används tooperform lookup- eller toocorrelate med din dataström. toomake användningen av referensdata i ditt Azure Stream Analytics-jobb vanligtvis använder du en [referens Data ansluta](https://msdn.microsoft.com/library/azure/dn949258.aspx) i frågan. Stream Analytics använder Azure Blob storage som hello lagringsskikt för referensdata med Azure Data Factory referens data kan vara omvandlade och/eller kopierade tooAzure Blob storage för användning som referensdata från [valfritt antal molnbaserade och lokalt datalager](../data-factory/data-factory-data-movement-activities.md). Referensdata modelleras som en serie blobbar (definieras i hello inkommande configuration) i stigande ordning efter hello tidsvärdet anges i hello blob-namnet. Den **endast** har stöd för att lägga till toohello slutet av hello sekvens med hjälp av ett datum/tid **större** än hello som anges av hello senaste blob i hello sekvens.

Stream Analytics har en **gränsen på 100 MB per blob** men jobb kan bearbeta flera referens blobbar med hjälp av hello **sökvägar** egenskapen.


## <a name="configuring-reference-data"></a>Konfigurera referensdata
tooconfigure din referensdata måste du först toocreate indata som är av typen **referensdata**. hello tabellen nedan beskriver varje egenskap som du behöver tooprovide när du skapar hello referensindata med beskrivningen:


<table>
<tbody>
<tr>
<td>Egenskapsnamn</td>
<td>Beskrivning</td>
</tr>
<tr>
<td>Ett inmatat Alias</td>
<td>Ett eget namn som ska användas i hello jobbet frågan tooreference detta indata.</td>
</tr>
<tr>
<td>Lagringskonto</td>
<td>hello namnet på hello storage-konto där dina blobbar finns. Om den är i hello samma prenumeration som Stream Analytics-jobbet kan välja hello i listrutan.</td>
</tr>
<tr>
<td>Lagringskontonyckel</td>
<td>hello hemlig nyckel kopplad till hello storage-konto. Detta hämtar fylls automatiskt i om hello storage-konto i hello samma prenumeration som Stream Analytics-jobbet.</td>
</tr>
<tr>
<td>Lagringsbehållaren</td>
<td>Behållare innehåller en logisk gruppering för blobbar som lagras i hello Microsoft Azure Blob-tjänsten. När du överför en blob-toohello Blob-tjänsten måste du ange en behållare för blobben.</td>
</tr>
<tr>
<td>Sökvägar</td>
<td>hello sökväg används toolocate dina blobbar i hello angivna behållaren. Du kan välja en eller flera instanser av följande 2 variabler hello toospecify inom hello-sökväg:<BR>{date} {time}<BR>Exempel 1: products/{date}/{time}/product-list.csv<BR>Exempel 2: products/{date}/product-list.csv
</tr>
<tr>
<td>[Valfritt] datumformat</td>
<td>Om du har använt {date} inom hello sökvägar som du angav, kan du välja hello datumformat där dina blobbar ordnas hello listrutan av de format som stöds.<BR>Exempel: ÅÅÅÅ-MM/DD, åååå-MM-DD osv.</td>
</tr>
<tr>
<td>[Valfritt] tidsformat</td>
<td>Om du har använt {time} inom hello sökvägar som du angav, kan du välja hello tidsformat som dina blobbar ordnas hello listrutan av de format som stöds.<BR>Exempel: HH, HH/mm eller HH-mm</td>
</tr>
<tr>
<td>Händelsen serialiseringsformat</td>
<td>toomake att dina frågor fungerar hello som du tänkt Stream Analytics måste tooknow vilka serialiseringsformat du använder för inkommande dataströmmar. För referensdata är hello stöds formaten CSV och JSON.</td>
</tr>
<tr>
<td>Encoding</td>
<td>UTF-8 är hello stöds endast kodningsformat just nu</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generera referensdata enligt ett schema
Om din referensdata är en långsamt föränderliga datauppsättning, är stöd för att uppdatera referensdata aktiverat genom att ange en sökväg mönster i hello inkommande konfigurationen med hjälp av hello {date} och {time} ersättning token. Stream Analytics hämtar hello uppdateras referens datadefinitioner baserat på den här sökvägen. Till exempel ett mönster för `sample/{date}/{time}/products.csv` med datumformatet **”åååå-MM-DD”** och ett tidsformat för **”HH-mm”** instruerar Stream Analytics toopick in hello uppdateras blob `sample/2015-04-16/17-30/products.csv` kl: 30 på April 16, 2015 UTC-tidszonen.

> [!NOTE]
> För närvarande leta Stream Analytics-jobb efter hello blob uppdatering endast när hello datortiden avancerar toohello tid kodats i hello blob-namnet. Till exempel hello jobbet söker efter `sample/2015-04-16/17-30/products.csv` så snart som möjligt, men ingen tidigare än 5:30 PM 16 April 2015 UTC tid zonen. Kommer det att *aldrig* leta efter en blob med ett kodat tidigare än hello sista som har identifierats.
> 
> T.ex. när jobbet hello hittar hello blob `sample/2015-04-16/17-30/products.csv` kommer att ignorera alla filer som har ett kodat infaller tidigare än 5:30 PM 16 April 2015 så om en sen anländer `sample/2015-04-16/17-25/products.csv` blob skapas i hello samma behållare hello jobbet kommer inte att användas.
> 
> Likaså om `sample/2015-04-16/17-30/products.csv` skapas endast klockan 10:03 16 April 2015 men ingen blob med ett tidigare datum finns i hello behållare, hello jobbet kommer att använda den här filen från klockan 10:03 16 April 2015 och använda hello tidigare referensdata fram till dess.
> 
> Ett undantag toothis är när hello jobbet måste toore processdata bakåt i tiden eller hello jobbet är första gången. Vid start söker hello jobbet efter hello senaste blob innan hello jobbet starttid har angetts. Detta görs tooensure att det finns en **icke-tom** refererar till datauppsättningen när hello jobbet startar. Om något inte går att hitta hello jobbet visar hello följande diagnostiska: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kan vara används tooorchestrate hello arbete skapar hello uppdateras blobbar som krävs av Stream Analytics tooupdate referens datadefinitioner. Data Factory är en molnbaserad integration datatjänst som samordnar och automatiserar hello flytt och transformering av data. Data Factory stöder [ansluter tooa många molnbaserade och lokala datalager](../data-factory/data-factory-data-movement-activities.md) och enkelt flytta data enligt ett schema som du anger. Mer information och steg-för-steg-anvisningar om hur tooset upp en Datafabrik pipeline toogenerate referensdata för Stream Analytics som uppdateras enligt ett fördefinierat schema kolla detta [GitHub exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tips om att uppdatera referensdata
1. Skriver över referens data-blobs inte ska orsaka Stream Analytics tooreload hello blob och i vissa fall kan det orsaka hello jobbet toofail. hello rekommenderat sätt toochange referensdata tooadd en ny blob med samma mönster för behållaren och sökvägen som definierats i hello jobbet indata hello och använda en datum/tid **större** än hello som anges av hello senaste blob i hello sekvens.
2. Referens för data-blobs är **inte** sorterade efter hello blob ”senast ändrad” tid men bara om hello datum och tid som anges i hello blob-namn med hjälp av hello {date} och {time} ersättningar.
3. På några tillfällen ett jobb måste gå tillbaka i tiden, därför referens data-blobs måste inte kan ändras eller tas bort.

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
Du har varit introducerades tooStream Analytics, en hanterad tjänst för streaming analytics på data från hello Sakernas Internet. toolearn mer om den här tjänsten finns i:

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
