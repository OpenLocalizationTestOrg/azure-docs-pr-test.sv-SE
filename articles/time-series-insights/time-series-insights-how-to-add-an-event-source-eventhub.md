---
title: "aaaHow tooadd en Händelsehubb händelse källa tooyour Azure tid serien Insights miljö | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur tooadd en händelse av datakälla som är anslutna tooan Event Hub tooyour tid serien insikter miljö"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: a98cd91deb51c829084a00c5f2a80b39d0fc511c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-event-hub-event-source"></a>Hur tooadd en Händelsehubb händelsekälla

Den här självstudiekursen beskrivs hur hello toouse Azure portal tooadd en händelsekälla läser från en Händelsehubb tooyour tid serien insikter miljö.

## <a name="prerequisites"></a>Krav

Du har skapat en Händelsehubb och skriver tooit händelser. Mer information om Händelsehubbar finns <https://azure.microsoft.com/services/event-hubs/>

> [Konsumentgrupper] Varje gång serien insikter händelsekälla måste toohave sin egen dedikerad konsumentgrupp som inte delas med andra användare. Om flera läsare förbrukar händelser från Hej samma konsumentgrupp, alla läsare är sannolikt toosee fel. Observera att det finns också en gräns på 20 konsumentgrupper per Event Hub. Mer information finns i hello [Programmeringsguide för Event Hubs](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Välj ett alternativ

hello inställningar för hello händelsekälla kan anges manuellt eller en händelsehubb kan väljas från hello händelsehubbar som är tillgängliga tooyou.
I hello **importalternativ** selector, väljer du något av följande alternativ för hello:

* Ange Event Hub-inställningar manuellt
* Använd Händelsehubb från tillgängliga prenumerationer

### <a name="select-an-available-event-hub"></a>Välj en tillgänglig Händelsehubb

hello i tabellen nedan beskrivs varje alternativ hello ny händelsekälla på fliken med dess beskrivning när du väljer en tillgänglig Händelsehubb som en händelsekälla:

| EGENSKAPSNAMN | BESKRIVNING |
| --- | --- |
| Händelsekällans namn | hello namnet på din händelsekälla. Det här namnet måste vara unikt i din miljö för tid serien insikter.
| Källa | Välj **Händelsehubb** toocreate en Händelsehubb händelsekälla.
| Prenumerations-Id | Välj hello prenumeration som den här händelsehubben skapades.
| Service bus-namnrymd | Välj hello Service Bus-namnrymd som innehåller hello Event Hub.
| Namnet på händelsehubben | Välj hello hello Event Hub.
| Namnet på händelsehubben princip | Välj hello delad åtkomstprincip som kan skapas på hello Event Hub konfigurera fliken. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar. hello delad åtkomstprincip för din händelsekälla *måste* har **läsa** behörigheter.
| Event hub konsumentgrupp | Hej konsumentgrupp tooread händelser från hello Event Hub. Det är starkt rekommenderat toouse en dedikerad konsumentgrupp för din händelsekälla.

### <a name="provide-event-hub-settings-manually"></a>Ange Event Hub-inställningar manuellt

hello i tabellen nedan beskrivs varje egenskap i hello ny händelsekälla flik med dess beskrivning när du anger inställningarna manuellt:

| EGENSKAPSNAMN | BESKRIVNING |
| --- | --- |
| Händelsekällans namn | hello namnet på din händelsekälla. Det här namnet måste vara unikt i din miljö för tid serien insikter.
| Källa | Välj **Händelsehubb** toocreate en Händelsehubb händelsekälla.
| Prenumerations-Id | hello-prenumeration som den här händelsehubben skapades.
| Resursgrupp | hello-prenumeration som den här händelsehubben skapades.
| Service bus-namnrymd | En Service Bus-namnrymd är en behållare för en uppsättning meddelandeentiteter. När du har skapat en ny Händelsehubb skapade du även en Service Bus-namnrymd.
| Namnet på händelsehubben | hello namnet på din Event Hub. När du skapade din händelsehubb gav du den även ett specifikt namn
| Namnet på händelsehubben princip | Hej princip för delad åtkomst, som kan skapas på hello Event Hub konfigurera fliken. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar. hello delad åtkomstprincip för din händelsekälla *måste* har **läsa** behörigheter.
| Event hub principnyckel | hello delade åtkomstnyckeln används tooauthenticate åtkomst toohello Service Bus-namnrymd. Ange hello primära och sekundära nycklarna här.
| Event hub konsumentgrupp | Hej konsumentgrupp tooread händelser från hello Event Hub. Det är starkt rekommenderat toouse en dedikerad konsumentgrupp för din händelsekälla.

## <a name="next-steps"></a>Nästa steg

1. Lägg till en miljö med data access princip tooyour [Definiera principer för dataåtkomst](time-series-insights-data-access.md)
1. Åtkomst till din miljö i hello [tid serien Insights-portalen](https://insights.timeseries.azure.com)
