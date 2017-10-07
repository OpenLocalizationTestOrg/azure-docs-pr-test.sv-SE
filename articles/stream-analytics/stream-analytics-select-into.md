---
title: "aaaDebug Azure Stream Analytics-frågor med SELECT INTO | Microsoft Docs"
description: "Data efter halva exempelfråga med SELECT INTO-instruktioner i Stream Analytics"
keywords: 
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 27e41af1a6ea06b4509d07a3a67087490d0ec1fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a>Felsöka frågor med hjälp av SELECT INTO-instruktioner

I realtid databearbetning verkar veta vilka hello-data hello mitten av hello frågan kan vara användbart. Eftersom indata- och stegen för ett Azure Stream Analytics-jobb kan läsa flera gånger, kan du skriva extra SELECT INTO-instruktioner. Gör det matar ut mellanliggande data till lagring och du kan inspektera hello är korrekt hello data, precis som *titta på variabler* göra när du felsöker ett program.

## <a name="use-select-into-toocheck-hello-data-stream"></a>Använd SELECT INTO toocheck hello dataström

hello har följande exempelfråga i Azure Stream Analytics-jobbet en strömindata, två referens indata och en utgående tooAzure Table Storage. hello frågan kopplar ihop data från hello händelsehubb och två blobbar tooget hello namn och kategori Referensinformation:

![SELECT INTO exempelfråga](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Observera att hello jobbet körs, men inga händelser genereras i hello utdata. På hello **övervakning** ikonen som visas här, du kan se att hello indata ger data, men du inte vet vilka steg i hello **ansluta** orsakade alla hello händelser toobe bort.

![hello övervakning sida vid sida](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
I det här fallet kan du lägga till några extra SELECT INTO-instruktioner för ”loggar” hello mellanliggande kopplingsresultatet och hello data som läses från hello indata.

I det här exemplet har vi lagt till två nya ”tillfällig utdata”. De kan vara alla mottagare som du vill. Här kan vi använda Azure Storage som exempel:

![Lägga till extra SELECT INTO-instruktioner](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Du kan sedan skriva om frågan hello så här:

![Omskrivet SELECT INTO-frågan](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Nu startar hello jobbet igen och låt den kör några minuter. Frågan temp1 och temp2 med Visual Studio Cloud Explorer tooproduce hello följande tabeller:

**temp1 tabell**
![SELECT INTO temp1 tabell](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**temp2 tabell**
![SELECT INTO temp2 tabell](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Som du kan se temp1 och temp2 har data och hello namnkolumnen fylls korrekt i temp2. Men eftersom det fortfarande finns inga data i utdata, det är något fel:

![SELECT INTO output1 tabell utan data](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Hello datasampling du som kan vara nästan säkert att hello problemet är hello andra koppling. Du kan hämta hello referensdata från hello blob och titta:

![SELECT INTO ref-tabell](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Som du ser skiljer hello format hello GUID i den här referensdata sig från hello [från]-kolumnen i temp2 hello format. Det är därför hello data inte kommer fram output1 som förväntat.

Du kan åtgärda hello dataformat, ladda upp den tooreference blob och försök igen:

![SELECT INTO temporära tabellen](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Den här gången hello data i hello utdata formateras och fyllts som förväntat.

![SELECT INTO slutliga tabell](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Få hjälp

För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg

* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

