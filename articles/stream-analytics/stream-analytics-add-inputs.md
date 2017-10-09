---
title: aaaAdd data indata tooyour Stream Analytics-jobb | Microsoft Docs
description: "Lär dig hur toohook upp en datakälla tooyour Stream Analytics jobbet som strömmande data i indata från Händelsehubbar eller referens data från blogg lagring."
keywords: "indata, strömmande data"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 674000268fcdf9bc000af3e2f166cb66f1366922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-tooa-stream-analytics-job"></a>Lägg till en strömmande data indata eller referens data tooa Stream Analytics-jobbet
Lär dig hur toohook upp en datakälla tooyour Stream Analytics-jobbet som strömmande data i indata från Händelsehubbar eller referens data från Blob storage.

Azure Stream Analytics-jobb kan vara anslutna tooone indata eller mer, som definierar en anslutning tooan befintlig datakälla. När data skickas toothat datakälla, är det används av hello Stream Analytics-jobbet och behandlas i realtid som strömmande data. Stream Analytics har förstklassigt integrering med [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) och [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) både inom och utanför hello jobbet prenumeration.

Den här artikeln är ett steg i hello [Stream Analytics-Utbildningsväg](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Indata: data och referensdata
Det finns två olika typer av indata i Stream Analytics: dataströmmar och referensdata.

* **Dataströmmar**: Stream Analytics-jobb måste innehålla minst en data dataströmmen inkommande toobe konsumeras och transformeras av hello jobb. Azure Blob storage och Azure Event Hubs kan användas som indata datakällor för dataströmmen. Händelsehubbar i Azure är används toocollect händelseströmmar från anslutna enheter, tjänster och program. Azure Blob storage kan användas som en Indatakällan för att föra in stora mängder data som en dataström.  
* **Referensdata**: Stream Analytics stöder en andra typen av extra inkommande kallas referensdata.  Dessa data är statiska eller sakta ändra som skillnad från toodata i rörelse.  Den används vanligtvis för att utföra look-ups och samband med dataströmmar toocreate en större mängd data.  Azure Blob storage är för närvarande stöds endast hello Indatakällan för referensdata.  

tooadd ett inkommande tooyour Stream Analytics-jobb:

1. I hello Azure-portalen klickar du på **indata** och klicka sedan på **lägga till indata** i Stream Analytics-jobbet.
   
    ![Klassiska Azure-portalen – lägga till indata.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    I hello Azure-portalen klickar du på hello **indata** panelen i Stream Analytics-jobbet.  
   
    ![Azure portal – lägga till indata.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. Ange hello hello indata: antingen **dataströmmen** eller **referensdata**.
   
    ![Lägga till hello korrekta data indata, strömmas eller refererar till](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Lägga till hello korrekta data indata, strömmas eller refererar till](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. Om du skapar en dataström inmatning, ange hello källtyp för hello indata.  Det här steget kan hoppas över när referensdata skapas som endast Blob-lagring stöds just nu.
   
    ![Lägg till inkommande dataström data](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Lägg till inkommande dataström data](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. Ange ett eget namn för den här inkommande i hello inmatat Alias rutan.  Det här namnet används i jobbfråga senare på toorefer toohello indata.
   
    Fyll i hello resten av hello behövs egenskaper tooconnect tooyour datakälla. De här fälten varierar beroende på typ av indata- och och definieras i detalj [här](stream-analytics-create-a-job.md).  
   
    ![Lägga till event hub data indata](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. Ange inställningar för hello-serialisering för hello indata:
   
   * toomake att dina frågor fungerar hello som du tänkt ange hello **händelse serialiseringsformat** för inkommande data.  Stöds serialisering format är JSON-, CSV- och Avro.
   * Kontrollera hello **kodning** för hello data.  UTF-8 är hello stöds endast kodningsformat just nu.
     
     ![Inställningarna för serialisering för hello indata](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Inställningarna för serialisering för hello indata](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. När du har slutfört hello inkommande skapa verifierar Stream Analytics att den kan ansluta toohello Indatakällan.  Du kan visa hello status för hello Testa anslutning igen i hello Notification hub.
   
    ![Testa anslutningen för hello strömning indata](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Testa anslutningen för hello strömning indata](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Få hjälp med streaming indata
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

