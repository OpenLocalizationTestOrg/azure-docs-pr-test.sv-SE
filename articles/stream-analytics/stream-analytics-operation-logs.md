---
title: "aaaDebug med hjälp av åtgärden och service i Stream Analytics | Microsoft Docs"
description: "Hur toouse Stream Analytics åtgärdsloggar"
keywords: "tjänstloggar"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: d3dd27706ccc879a724e1894b33d47021d972f31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Felsöka Stream Analytics-jobb med hjälp av loggar och drift
Alla Azure-tjänster ange operativa loggning meddelanden toousers toorecord uppgifter relaterade toomanagement åtgärder. Den här informationen kan användas för felsökning, till exempel visa jobbstatus och jobbförloppet fel meddelanden tootrack hello förloppet för ett jobb med tiden från start tooprocessing toooutput i Azure Stream Analytics.

## <a name="find-operation-logs-in-hello-azure-management-portal"></a>Hitta åtgärden loggas i hello Azure Management portal
Åtgärdsloggar kan nås på två sätt:  

* Instrumentpanelen i hello Stream Analytics-jobbet  
* Tjänster i hello Azure klassiska portal  

## <a name="dashboard-of-hello-stream-analytics-job"></a>Instrumentpanelen i hello Stream Analytics-jobbet
En länk toohello motsvarande loggar för ett Stream Analytics-jobb visas på fliken instrumentpanelen i hello jobb. Om du klickar på länken anges hello filter på ett sätt att den visar senaste loggar för det specifika jobbet.

  ![Välj Management Services-loggar](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Hanteringstjänster
toomanually navigera toohello Åtgärdsloggar för Stream Analytics och andra tjänster i hello Azure klassiska portal:

1. Klicka på **hanteringstjänster** i hello [Azure klassiska portal](https://manage.windowsazure.com).
2. Välj **Stream Analytics** för **typen** och hello namnet på hello jobb för **tjänstnamnet**.  
   
   ![Välj Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-hello-azure-portal"></a>Hitta granskningsloggar i hello Azure-portalen
toofind operativa loggar för Stream Analytics-jobbet i hello Azure-portalen klickar du på **Bläddra** och välj sedan **granskningsloggar**.

  ![Azure portal väljer Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Då öppnas ett blad som visar händelser från hello senaste 7 dagarna för alla resurser i din prenumeration.  Du kan filtrera toosee händelser av en ange typ eller tidsintervall genom att klicka på hello **Filter** kommando.

  ![Azure portal väljer Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Hämta logginformation om
Du kan filtrera efter Status tooview hello loggar och tidsintervall för jobbet.

Klicka på hello i hello Azure Management portal **information** knappen längst ned hello hello fönstret tooview mer information om en markerad händelse. 

  ![Välj information](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

I hello hello Azure-portalen klickar du på en logg post toosee detaljerade händelser i den.

  ![Azure portal väljer information](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Därifrån kan du öppna hello **detaljer** bladet genom att klicka på hello-händelse.

  ![Azure portal väljer information](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Felsöka ett jobb som misslyckades
Klicka på sökikonen hello i hello Azure-hanteringsportalen och Skriv ”misslyckades”. Detta ger ett resultat av alla loggar med fel. 

  ![Felsökning av ett jobb som misslyckades](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

I hello Azure-portalen, kan du filtrera efter nivå av meddelandet tooview **kritisk** händelser.

  ![Felsökning i Azure portal](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Du kan välja en hello fel och klicka på hello **information** för mer information om hello-fel.  Somliga felmeddelanden innehåller också information om hur toomitigate hello problemet. 

  ![Åtgärdsinformation](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Om du behöver toocontact [stöd](https://azure.microsoft.com/support/options/) eller ange information toohello team via hello [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), särskilt Observera hello driftsinformation hello **Korrelations-ID**. 

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

