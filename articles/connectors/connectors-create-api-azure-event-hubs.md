---
title: "aaaSet in Händelseövervakare med Händelsehubbar i Azure för Logikappar i Azure | Microsoft Docs"
description: "Övervaka dataströmmar tooreceive händelser och skicka händelser för Logikappar i Azure med Azure Event Hubs"
services: logic-apps
keywords: "dataströmmen, övervakning, händelsehubbar"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: 4aad2c2ac1134b4d4d440019b4773559e49be122
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-receive-and-send-events-with-hello-event-hubs-connector"></a>Övervaka, ta emot och skicka händelser med hello Händelsehubbar connector

tooset upp en Händelseövervakare så att din logikapp kan identifiera händelser, ta emot händelser och skicka händelser, ansluta tooan [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) från din logikapp. Lär dig mer om [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Krav

* Du har toohave en [Händelsehubbar namnområde och Event Hub](../event-hubs/event-hubs-create.md) i Azure. Läs [hur toocreate en Händelsehubbar namnområde och Event Hub](../event-hubs/event-hubs-create.md). 

* toouse [alla anslutningar](https://docs.microsoft.com/azure/connectors/apis-list) i din logikapp, har du toocreate en logikapp först. Läs [hur toocreate en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-hello-connection-string"></a>Kontrollera behörigheter för Händelsehubbar namnområde och hitta hello-anslutningssträng

För dina logic app tooaccess tjänsten något du har toocreate en [ *anslutning* ](./connectors-overview.md) mellan logik appen och hello tjänsten om du inte redan har gjort. Den här anslutningen auktoriserar tooaccess för logik appdata.
Din logik app tooaccess din Händelsehubb har toohave **hantera** behörigheter och hello anslutningssträngen för namnområdet Händelsehubbar.

toocheck dina behörigheter och hämta hello anslutningssträngen, Följ dessa steg.

1.  Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen"). 

2.  Gå tooyour Händelsehubbar *namnområde*, inte hello specifik Händelsehubb. Hello namnområde bladet under **inställningar**, Välj **principer för delad åtkomst**. Under **anspråk**, kontrollera att du har **hantera** behörigheter för det namnområdet.

    ![Hantera behörigheter för namnområdet Event Hub](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  toocopy hello anslutningssträngen för hello Händelsehubbar namnområde, Välj **RootManageSharedAccessKey**. Nästa tooyour primära nyckeln anslutningssträngen, Välj hello kopia.

    ![Kopiera anslutningssträngen för Händelsehubbar namnområde](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > tooconfirm om anslutningssträngen är associerad med Händelsehubbar namnområdet eller med en specifik Händelsehubb Kontrollera hello anslutningssträngen för hello `EntityPath` parameter. Om du hittar den här parametern hello anslutningssträngen är för en specifik Händelsehubb ”enhet” och är inte hello rätt sträng toouse med din logikapp.

4.  Nu när du uppmanas ange autentiseringsuppgifter när du lägger till en Händelsehubbar utlösare eller åtgärd tooyour logikapp, kan du ansluta tooyour Händelsehubbar namnområde. Namnge din anslutning, ange hello anslutningssträng som du kopierade och välj **skapa**.

    ![Ange anslutningssträng för Händelsehubbar namnområde](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    När du har skapat din anslutning ska hello anslutningsnamn visas i hello Händelsehubbar utlösare eller åtgärd. 
    Du kan sedan fortsätta med hello andra steg i din logikapp.

    ![Event Hubs namnområde anslutning som skapats](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Starta arbetsflödet när din Event Hub tar emot nya händelser

En [ *utlösaren* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) är en händelse som startar ett arbetsflöde i din logikapp. Följ dessa steg för att lägga till hello utlösare som identifierar den här händelsen om du vill starta ett arbetsflöde när nya händelser skickas tooyour Event Hub.

1.  I hello [Azure-portalen](https://portal.azure.com "Azure-portalen"), gå tooyour befintliga logikapp eller skapa en tom logikapp.

2.  Skriv i sökrutan för hello logik App Designer hello `event hubs` för filtret. Välj den här utlösaren: **när händelser är tillgängliga i Event Hub**

    ![Välj utlösare för när din Event Hub tar emot nya händelser](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Om du inte redan har en anslutning tooyour Händelsehubbar namnrymd, uppmanas du toocreate nu den här anslutningen. Namnge din anslutning och ange hello anslutningssträng för namnområdet Händelsehubbar. 
    Lär dig mer om det behövs [hur toofind anslutningssträngen](#permissions-connection-string).

    ![Ange anslutningssträng för Händelsehubbar namnområde](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    När du har skapat hello anslutning hello inställningar för hello **när en händelse i finns i en Händelsehubb** utlösaren visas.

    ![Med hjälp av inställningar för när din Event Hub tar emot nya händelser](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Ange eller välj hello namn för hello Event Hub som du vill toomonitor. Välj hello frekvensen och intervall för hur ofta du vill att toocheck hello Event Hub.

    > [!TIP]
    > toooptionally markerar du en konsumentgrupp för att läsa händelser, väljer **visa avancerade alternativ**. 

    ![Ange Händelsehubb eller konsumentgrupp](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    Du har nu skapat en utlösare toostart ett arbetsflöde för din logikapp. 
    Din logikapp kontrollerar hello angetts Event Hub baserat på hello-schema som du har angett. 
    Om din app hittar nya händelser i hello Event Hub, kör hello trigger andra åtgärder eller utlösare i din logikapp.

## <a name="send-events-tooyour-event-hub-from-your-logic-app"></a>Skicka händelser tooyour Händelsehubb från din logikapp

En [*åtgärd*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) är en aktivitet som utförs av logikapparbetsflödet. När du lägger till en utlösare tooyour logikapp, du kan lägga till en åtgärd tooperform åtgärder med data som genereras av den här utlösaren. toosend händelse-tooyour Händelsehubb från din logikapp, Följ dessa steg.

1.  I logik App Designer under din logik apputlösare väljer **nytt steg** > **lägga till en åtgärd**.

    ![Sedan Välj ”nytt steg”, ”Lägg till en åtgärd”](./media/connectors-create-api-azure-event-hubs/add-action.png)

    Nu kan du hitta och välja en åtgärd tooperform. 
    Men du kan välja en åtgärd, t.ex, vill vi hello Händelsehubbar åtgärd toosend händelser.

2.  Skriv i sökrutan hello `event hubs` för filtret.
Välj den här åtgärden: **överföringshändelse**

    ![Välj ”Händelsehubbar - överföringshändelse” åtgärd](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Ange hello krävs information om hello händelse, till exempel hello namn för hello Event Hub där du vill att toosend hello händelsen. Ange andra valfria information om hello-händelse, t.ex innehåll för händelsen.

    > [!TIP]
    > toooptionally ange hello Event Hub partition där toosend hello-händelse, Välj **visa avancerade alternativ**. 

    ![Ange Event Hub-namn och valfri händelseinformation](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Spara ändringarna.

    ![Spara din logikapp](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    Du har nu skapat en åtgärd toosend händelser från din logikapp. 

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/eventhubs/). 

## <a name="get-help"></a>Få hjälp

tooask frågor, besvara frågor och se vilka andra användare i Azure Logic Apps gör finns hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Logic Apps användaren feedbackwebbplats](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nästa steg

*  [Sök efter andra kopplingar till Azure logikappar](./apis-list.md)