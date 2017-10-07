---
title: "aaaHow tooadd en IoT-hubb händelse källa tooyour Azure tid serien Insights miljö | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur tooadd en händelse datakälla som är anslutna tooan IoT-hubb tooyour tid serien insikter miljö"
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
ms.openlocfilehash: c626f9653d1c012360120fa9fc3d211d7d5beb5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-iot-hub-event-source"></a>Hur tooadd en IoT-hubb händelsekälla

Den här självstudiekursen beskrivs hur hello toouse Azure portal tooadd en händelsekälla läser från en IoT-hubb tooyour tid serien insikter miljö.

## <a name="prerequisites"></a>Krav

Du har skapat en IoT-hubb och skriver tooit händelser. Mer information om IoT-hubbar finns <https://azure.microsoft.com/services/iot-hub/>

> [Konsumentgrupper] Varje gång serien insikter händelsekälla måste toohave sin egen dedikerad konsumentgrupp som inte delas med andra användare. Om flera läsare förbrukar händelser från Hej samma konsumentgrupp, alla läsare är sannolikt toosee fel. Mer information finns i hello [IoT-hubb Utvecklarhandbok](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Välj ett alternativ

hello inställningar för hello händelsekälla kan anges manuellt eller en IoT-hubb kan väljas från hello IoT-hubbar som är tillgängliga tooyou.
I hello **importalternativ** selector, väljer du något av följande alternativ för hello:

* Ange inställningar för IoT-hubb manuellt
* Använd IoT-hubb från tillgängliga prenumerationer

### <a name="select-an-available-iot-hub"></a>Välj en tillgänglig IoT-hubb

hello i tabellen nedan beskrivs varje alternativ hello ny händelsekälla på fliken med dess beskrivning när du väljer en tillgänglig IoT-hubb som en händelsekälla:

| EGENSKAPSNAMN | BESKRIVNING |
| --- | --- |
| Händelsekällans namn | hello namnet på din händelsekälla. Det här namnet måste vara unikt i din miljö för tid serien insikter.
| Källa | Välj **IoT-hubb** toocreate en IoT-hubb händelsekälla.
| Prenumerations-Id | Välj hello prenumeration där den här IoT-hubben har skapats.
| IoT-hubbnamnet | Välj hello hello IoT-hubb.
| Principnamn för IoT-hubb | Välj hello delad åtkomstprincip som finns på hello IoT-hubb på fliken Inställningar. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar. hello delad åtkomstprincip för din händelsekälla *måste* har **tjänsten ansluta** behörigheter.
| Konsumentgrupp för IoT-hubb | Hej konsumentgrupp tooread händelser från hello IoT-hubb. Det är starkt rekommenderat toouse en dedikerad konsumentgrupp för din händelsekälla.

### <a name="provide-iot-hub-settings-manually"></a>Ange inställningar för IoT-hubb manuellt

hello i tabellen nedan beskrivs varje egenskap i hello ny händelsekälla flik med dess beskrivning när du anger inställningarna manuellt:

| EGENSKAPSNAMN | BESKRIVNING |
| --- | --- |
| Händelsekällans namn | hello namnet på din händelsekälla. Det här namnet måste vara unikt i din miljö för tid serien insikter.
| Källa | Välj **IoT-hubb** toocreate en IoT-hubb händelsekälla.
| Prenumerations-Id | hello prenumeration där den här IoT-hubben har skapats.
| Resursgrupp | hello prenumeration där den här IoT-hubben har skapats.
| IoT-hubbnamnet | hello namnet på din IoT-hubb. När du skapade din IoT-hubb gav du den även ett specifikt namn
| Principnamn för IoT-hubb | hello delad åtkomstprincip som kan skapas på hello IoT-hubb på fliken Inställningar. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar. hello delad åtkomstprincip för din händelsekälla *måste* har **tjänsten ansluta** behörigheter.
| IoT-hubb principnyckel | hello delade åtkomstnyckeln används tooauthenticate åtkomst toohello Service Bus-namnrymd. Ange hello primära och sekundära nycklarna här.
| Konsumentgrupp för IoT-hubb | Hej konsumentgrupp tooread händelser från hello IoT-hubb. Det är starkt rekommenderat toouse en dedikerad konsumentgrupp för din händelsekälla.

## <a name="next-steps"></a>Nästa steg

1. Lägg till en miljö med data access princip tooyour [Definiera principer för dataåtkomst](time-series-insights-data-access.md)
1. Åtkomst till din miljö i hello [tid serien Insights-portalen](https://insights.timeseries.azure.com)
