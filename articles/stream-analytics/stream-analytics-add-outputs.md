---
title: "aaaHow tooconfigure data matar ut för Stream Analytics-jobb | Microsoft Docs"
description: "Konfigurera utdata för Stream Analytics-jobb | Learning sökvägssegment."
keywords: data som utdata, dataflyttning
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/26/2017
ms.author: samacha
ms.openlocfilehash: c5d89e9e9f9211d3e778580c071dd53d56aed9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-outputs-for-stream-analytics-jobs"></a>Hur tooconfigure data matar ut för Stream Analytics-jobb

Azure Stream Analytics-jobb kan vara anslutna tooone eller mer utdata som definierar en anslutning tooan befintliga data mottagare. Stream Analytics-jobbet, processer och omvandlar inkommande data som skrivs en dataström med data utdata händelser tooyour jobbutdata.

Utdata för Stream Analytics kan vara används toosource realtids-instrumentpaneler och aviseringar, utlösare data movement arbetsflöden eller helt enkelt Arkivera data för batch-bearbetning vid ett senare tillfälle. Stream Analytics har förstklassigt integrering med flera Azure-tjänster som dokumenteras här.

tooadd utdata tooyour Stream Analytics-jobbet:

1. I hello [Azure-portalen](https://portal.azure.com), öppna ditt jobb och klickar på **utdata** och klicka sedan på **Lägg till** i hello utdata bladet som visas.
   
    ![Lägga till utdata](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. Ange ett eget namn för den här utdata i hello **kolumnalias** rutan. Det här namnet kan användas i ditt jobb fråga senare på toorefer toohello utdata.  
   
    Fyll i hello resten av hello krävs anslutning egenskaper tooconnect tooyour utdata.  De här fälten varierar beroende på utdatatypen och definieras här.  
   
    ![Välj typ av data movement](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. Hello utdatatypen måste du använda toospecify hur hello data serialiseras eller formaterad. Här beskrivs hello specifika serialisering inställningar för varje Utdatatyp av.
   
    Fyll i hello resten av hello behövs egenskaper tooconnect tooyour datakälla. De här fälten varierar beroende på typ av indata- och och definieras i detalj i hello [skapa jobbet artikel](stream-analytics-create-a-job.md).  

> [!Note]
>
> Output-elementet tillagda toohello jobb, måste finnas innan hello jobbet har startats och händelser börjar flöda. Till exempel om du använder Blob storage som utdata hello jobbet kommer inte att skapa ett lagringskonto automatiskt. Den måste toobe som skapats av hello användare innan hello ASA jobb har startat.
> 
 

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

