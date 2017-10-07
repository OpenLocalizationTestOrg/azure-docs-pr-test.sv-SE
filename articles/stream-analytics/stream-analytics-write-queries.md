---
title: "aaaHow toowrite frågor i Stream Analytics | Microsoft Docs"
description: "Skriva frågor i Stream Analytics och fråga efter data | Learning sökvägssegment."
keywords: "hur toowrite frågor, fråga efter data skriva en fråga, skriva frågor"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: b943c34f10afd2b21789afbd341c471a5f168729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowrite-queries-in-stream-analytics"></a>Hur toowrite frågor i Stream Analytics
Skriva frågor för bearbetning av logiken i Azure Stream Analytics implementeras som en ”stående-fråga” som har definierats innan ett jobb startar och köras på data eftersom den når hello jobb. Hej Dataomvandling uttryckt i ett SQL-liknande frågespråk som i stort sett en delmängd av T-SQL med några läggs-tillägg som [fönsterhantering](https://msdn.microsoft.com/library/azure/dn835019.aspx) används tooexpress temporala semantik.

## <a name="writing-queries"></a>Skriva frågor:
1. I din Stream Analytics-jobbet i hello Azure Management portal, klickar du på **frågan**.
   
    ![Välj fråga](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    I hello Azure-portalen klickar du på **frågan**.
   
    ![Välj Query Preview](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. Nya jobb har en fråga mallen toohelp komma igång. hello frågan mallen utför en ”direktlagringsdisk” fråga att projekt alla fält från inkommande händelser i hello utdata.  
   
   * Om du har definierat minst ett indata och utdata för jobbet, kan du ersätta hello platshållare ”[YourOutputAlias]” och ”[YourInputAlias]” fält med hello-alias för hello indata och utdata som du vill använda först. Dessutom kan du fortfarande skapa och testa din fråga i hello klassiska Azure-portalen utan att definiera indata och utdata för hello jobb.
   * Om du vill tooperform mer bearbetning än en enkel direkt kan redigera du hello frågedefinitionen. tooget igång med redigering av frågan ta en titt på vissa vanlig fråga mönster fångas [här](stream-analytics-stream-analytics-query-patterns.md).  
   
   ![Fråga efter data fönster](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="toovalidate-query-data-is-working"></a>toovalidate fråga efter data fungerar:
Du kan testa att frågan fungerar som förväntat genom att köra det i hello webbläsare över en eller flera lokala JSON-filer som innehåller testdata. Det här inte starta hello jobbet eller ha någon faktureringsinformation konsekvenser.

> [!NOTE]
> För närvarande stöds webbläsarbaserad frågetestning inte i hello Azure-portalen.  
> 
> 

1. Kontrollera att det inte finns några fel i hello-fråga (annars hello testknappen inaktiveras) och klicka sedan på knappen för hello-Test.  
   
   ![Fråga efter data Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. Du kommer att tillfrågas toospecify filer för varje hello indata som refereras till i hello frågan. I det här exemplet hello mallen fråga lämnas som-är så hello dialogrutan uppmanar för indata med namnet ”yourinputalias”.  
   
   ![Testa fråga](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. Bläddra tooa Testa fil. Flera exempelfiler är tillgängliga på [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) och du kan också hämta exempeldata från din egen dataströmmen indata via hello exempeldata funktionen hello indata på fliken.  
   
   ![Frågan indata](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. När du har stängt dialogrutan hello frågan ska köras via hello testdata och du ser hello resultat på hello hello frågan sidans nederkant.  
   
   ![Frågesammanfattning av](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

