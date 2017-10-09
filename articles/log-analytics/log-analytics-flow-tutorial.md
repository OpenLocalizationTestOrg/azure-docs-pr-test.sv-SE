---
title: aaaAutomate Azure logganalys bearbetar med Microsoft Flow
description: "Lär dig hur du kan använda Microsoft Flow tooquickly automatisera repeterbara processer med hello Azure logganalys-anslutningen."
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: bwren
ms.openlocfilehash: 52026df968682842cc9f8d55f6f9875c5f9c10b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-log-analytics-processes-with-hello-connector-for-microsoft-flow"></a>Automatisera logganalys processer med hello connector för Microsoft Flow
[Microsoft Flow](https://ms.flow.microsoft.com) kan du toocreate automatiserade arbetsflöden med hjälp av hundratals åtgärder för en mängd olika tjänster. Utdata från en åtgärd kan användas som indata tooanother så att du toocreate integrering mellan olika tjänster.  hello Azure logganalys connector för Microsoft Flow Tillåt toobuild arbetsflöden som innehåller data som hämtats av loggen söker i logganalys.

Till exempel använda Microsoft Flow toouse logganalys data i ett e-postmeddelande från Office 365, skapa ett programfel i Visual Studio Team Services eller skicka en Slack meddelande.  Du kan utlösa ett arbetsflöde med ett enkelt schema eller från en åtgärd i en ansluten tjänst, till exempel när ett e-postmeddelande eller en tweet tas emot.  


> [!NOTE]
> hello Azure logganalys connector för Microsoft Flow kräver din arbetsyta toobe uppgraderas toohello nya logganalys frågespråk. Du lär dig mer om hello nytt språk och få hello proceduren tooupgrade ditt arbetsområde på [uppgradera sökningen Azure logganalys arbetsytan toonew loggen](log-analytics-log-search-upgrade.md).  

hello kursen i den här artikeln visar hur toocreate ett flöde som automatiskt skickar hello resultaten av en sökning för logganalys-loggen via e-post, bara ett exempel på hur du kan använda logganalys i Microsoft Flow. 


## <a name="step-1-create-a-flow"></a>Steg 1: Skapa ett flöde
1. Logga in för[Microsoft Flow](http://flow.microsoft.com), och välj **Mina flödar**.
2. Klicka på **+ skapa från tomt**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>Steg 2: Skapa en utlösare för din flöde
1. Klicka på **Sök hundratals kopplingar och utlösare**.
2. Typen **schema** i hello sökrutan.
3. Välj **schema**, och välj sedan **schema - upprepning**.
4. I hello **frekvens** markerar du kryssrutan **dag** och i hello **intervall** ange **1**.<br><br>![Dialogrutan för Microsoft Flow utlösare](media/log-analytics-flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>Steg 3: Lägg till en Log Analytics-åtgärd
1. Klicka på **+ nytt steg**, och klicka sedan på **lägga till en åtgärd**.
2. Sök efter **logga Analytics**.
3. Klicka på **Azure Log Analytics – Kör fråga och visualisera resultat**.<br><br>![Kör frågefönstret logganalys](media/log-analytics-flow-tutorial/flow02.png)

## <a name="step-4-configure-hello-log-analytics-action"></a>Steg 4: Konfigurera hello logganalys åtgärd

1. Ange hello information för din arbetsyta inklusive hello prenumerations-ID, resursgrupp, och arbetsytans namn.
2. Lägg till följande logganalys frågan toohello hello **frågan** fönster.  Detta är en exempelfråga och du kan ersätta med alla andra som returnerar data.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. Välj **HTML-tabell** för hello **diagramtyp**.<br><br>![Log Analytics-åtgärd](media/log-analytics-flow-tutorial/flow03.png)

## <a name="step-5-configure-hello-flow-toosend-email"></a>Steg 5: Konfigurera hello flödet toosend e-post

1. Klicka på **nytt steg**, och klicka sedan på **+ Lägg till en åtgärd**.
2. Sök efter **Office 365 Outlook**.
3. Klicka på **Office 365 Outlook – skicka ett e-post**.<br><br>![Urvalsfönster för Office 365 Outlook](media/log-analytics-flow-tutorial/flow04.png)

4. Ange hello e-postadress för en mottagare i hello **till** fönster och ett ämne för e-post för hello i **ämne**.
5. Klicka någonstans i hello **brödtext** rutan.  En **dynamiskt innehåll** öppnas med värden från tidigare åtgärder.  
6. Välj **brödtext**.  Detta är hello resultaten av hello frågan i hello logganalys-åtgärd.
6. Klicka på **visa avancerade alternativ**.
7. I hello **är HTML** väljer **Ja**.<br><br>![Fönster för Office 365 e-konfiguration](media/log-analytics-flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>Steg 6: Spara och testa din flöde
1. I hello **flöde namnet** , lägga till ett namn för din flöde, och klicka sedan **skapa flödet**.<br><br>![Spara flöde](media/log-analytics-flow-tutorial/flow06.png)
2. hello flödet skapas nu och kommer att köras efter en dag som är hello-schema som du angett. 
3. tooimmediately test hello flödet klickar du på **kör nu** och sedan **kör flödet**.<br><br>![Kör flöde](media/log-analytics-flow-tutorial/flow07.png)
3. Kontrollera hello e-post för hello mottagare som du angav när hello flödet har slutförts.  Du bör ha fått ett e-postmeddelande med en brödtext liknande toohello följande:<br><br>![E-postexempel](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [logga sökningar i logganalys](log-analytics-log-search-new.md).
- Lär dig mer om [Microsoft Flow](https://ms.flow.microsoft.com).



