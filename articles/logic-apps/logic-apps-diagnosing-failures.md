---
title: "fel när aaaDiagnose: Azure Logic Apps | Microsoft Docs"
description: "Vanliga sätt toounderstand där logikappar misslyckas"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 46d318625820034c95e6df3a71ab84c58f076dd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-logic-app-failures"></a>Diagnostisera logik app fel
Om det uppstår problem eller fel med logic apps kan det inte finns några metoder kan hjälpa dig att bättre förstå var hello fel kommer från.  

## <a name="azure-portal-tools"></a>Azure portal-verktyg
hello Azure-portalen innehåller många verktyg toodiagnose varje logikapp i varje steg.

### <a name="trigger-history"></a>Utlösaren historik

Varje logikapp har minst en utlösare. Om du märker att appar inte startar, titta först på hello utlösaren historik för mer information. Du kan komma åt hello utlösaren historik i hello logik app'ss huvudblad.

![Hitta hello utlösaren historik][1]

hello utlösaren Historik visar alla utlösare försök som görs i din logikapp. Du kan klicka på varje utlösare försök toodrill hello detaljer specifikt, någon inmatningar eller utdata som hello utlösaren försök skapas. Om du hittar misslyckade utlösare Markera hello utlösaren försök och välj hello **utdata** länka tooreview eventuella genererade felmeddelanden, till exempel FTP-autentiseringsuppgifter som inte är giltigt.

hello olika statusar som visas är:

* **Hoppade över**. hello endpoint var Avsökt toocheck för data och tog emot ett svar som att det fanns inga data.
* **Lyckades**. hello utlösaren emot svaret var data tillgängliga. Den här statusen kan bero på en manuell utlösare, en upprepning utlösare eller en avsökning utlösare. Denna status är oftast hello **Fired** status, men kanske inte om du har ett villkor eller SplitOn kommandot i kodvy som inte uppfylls.
* **Det gick inte**. Ett fel har genererats.

#### <a name="start-a-trigger-manually"></a>Starta en utlösare manuellt

Om du vill hello logik app toocheck för en utlösare som är tillgängliga omedelbart utan att vänta på hello nästa återkommande **Välj utlösare** på hello huvudblad tooforce en kontroll. Till exempel gör klickar på länken med en Dropbox-utlösare hello arbetsflöde tooimmediately avsökning Dropbox för nya filer.

### <a name="run-history"></a>Kör historik

Varje Eldad utlösare resulterar i en körning. Du kan komma åt information om körning från hello huvudblad som innehåller många detaljer som hjälper dig att förstå vad som hände under hello arbetsflöde.

![Hitta hello kör historik][2]

Körs visas något av följande statusar hello:

* **Lyckades**. Alla åtgärder har slutförts. Om ett fel uppstod har felet hanterats av en åtgärd som inträffat senare i hello arbetsflöde. Det vill säga hanterades hello fel av en åtgärd som har ställts in toorun efter en misslyckad åtgärd.
* **Det gick inte**. Minst en åtgärd hade fel som inte hanterades av en åtgärd senare i hello arbetsflöde.
* **Avbröt**. hello arbetsflödet kördes, men tog emot en begäran om avbrytning.
* **Kör**. hello arbetsflödet körs. Den här statusen kan uppstå för begränsad arbetsflöden eller på grund av hello aktuella priser plan. Mer information finns i [åtgärd gränserna för hello sida med priser](https://azure.microsoft.com/pricing/details/app-service/plans/). Konfigurera diagnostik (hello diagrammen som visas under hello kör historik) kan också ge information om begränsning av händelser som inträffar.

När du tittar på ett körningshistorik detaljer mer information.  

#### <a name="trigger-outputs"></a>Utlösaren utdata

Utlösaren matar ut visa hello data som kommer från hello utlösare. Dessa utdata kan hjälpa dig att avgöra om alla egenskaper returneras som förväntat.

> [!NOTE]
> Om du ser allt innehåll som du inte förstår lär du dig hur Azure Logic Apps [hanterar olika typer av innehåll](../logic-apps/logic-apps-content-type.md).
> 

![Utlösaren utdata-exempel][3]

#### <a name="action-inputs-and-outputs"></a>Åtgärden indata och utdata

Du kan detaljerat hello in- och utgångar som en åtgärd togs emot. Informationen är användbar för att förstå hello storlek och form hello utdata och för att söka efter eventuella felmeddelanden som kan ha genererats.

![Åtgärden indata och utdata][4]

## <a name="debug-workflow-runtime"></a>Felsöka arbetsflödets körtid

Tillsammans med övervakning hello indata, utdata och utlösare av en körning, kan du lägga till några steg tooa arbetsflöde som hjälp med felsökning. 
[RequestBin](http://requestb.in) är ett kraftfullt verktyg som du kan lägga till som ett steg i ett arbetsflöde. Du kan konfigurera en HTTP-begäran inspector toodetermine hello exakt storlek, form och format för en HTTP-begäran med hjälp av RequestBin. Du kan skapa en RequestBin och klistra in hello URL i en logikapp HTTP POST-åtgärd med brödtextinnehåll som du vill tootest, till exempel, ett uttryck eller en annan steg utdata. När du har kört hello logikapp kan du uppdatera din RequestBin toosee hur hello begäran bildades när genererats från hello Logic Apps-motorn.

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
