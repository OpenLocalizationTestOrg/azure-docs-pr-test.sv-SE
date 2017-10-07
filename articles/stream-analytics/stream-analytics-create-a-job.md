---
title: "aaaHow toocreate data analytics bearbetning jobb för Stream Analytics | Microsoft Docs"
description: "Skapa ett data analytics-jobb för bearbetning för Stream Analytics | Learning sökvägssegment."
keywords: analytics databearbetning
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: d4a3c89d8862d59688d06a1719b063efa2ab1c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-data-analytics-processing-job-for-stream-analytics"></a>Hur toocreate en analytics databearbetning jobbet för Stream Analytics
hello översta resurs i Azure Stream Analytics är en Stream Analytics-jobbet.  Det består av en eller flera ingående datakällor, en fråga uttrycka hello DTS och ett eller flera mål för utdata som resultat har skrivits till. Dessa aktivera tillsammans hello användaren tooperform dataanalys för strömmande data bearbetades scenarier.

toostart med Stream Analytics, börja med att skapa ett nytt Stream Analytics-jobb.  Observera att den här åtgärden har ingen fakturering effekter tills hello påbörjades.

1. Logga in på hello online [klassiska Azure-portalen](http://manage.windowsazure.com) eller hello [Azure-portalen](https://portal.azure.com/).
2. I hello portal: **klickar du på ny**, klicka på **datatjänster** eller **dataanalys** beroende på din portal och klicka sedan på **Azure Stream Analytics** eller **strömma Analytics** och sedan **Snabbregistrering**.
   
   ![Guiden analytics databearbetning](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Skapa dataanalys bearbetning jobbet](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Ange hello önskad konfiguration för hello Stream Analytics-jobbet.
   
   * I hello **jobbnamn** anger du ett namn tooidentify hello Stream Analytics-jobb. När hello **jobbnamn** har verifierats, visas en grön kryssmarkering i hello jobbnamn. Hej **jobbnamn** får bara innehålla alfanumeriska tecken och hello '-' tecken och måste vara mellan 3 och 63 tecken.
   * Använd **Region** i hello Azure-portalen eller **plats** i hello Azure portal toospecify hello geografiska plats där du vill toorun hello jobb.
   * Om du använder hello Azure-portalen, Välj eller skapa en storage-konto toouse som hello **Lagringskonto för Regional övervakning**. Det här lagringskontot är används toostore övervakningsdata för alla Stream Analytics-jobb som körs i den här regionen.
   * Om du använder hello Azure-portalen, ange en ny eller befintlig **resursgruppen** toohold relaterade resurser för ditt program.
4. När hello nya Stream Analytics-jobbalternativ har konfigurerats, klickar du på **skapa Stream Analytics-jobbet**. Det kan ta några minuter för hello Stream Analytics-jobbet toobe skapas. Du kan övervaka hello förlopp i hello meddelandehubben toocheck hello status.
   
   ![Data analytics bearbetning jobbet meddelandehubben](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Azure portal dataanalys bearbetar jobbet Skapa jobb](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. hello nytt jobb visas status för **Skapad**. Observera att hello **starta** är inaktiverat. Konfigurera hello jobbet indata-, fråge- och utdata innan du startar hello jobbet.
   
   ![Databearbetning analytics-jobbet Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portal dataanalys bearbetning jobbstatus](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

