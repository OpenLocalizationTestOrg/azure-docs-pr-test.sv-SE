---
title: aaaLearn hur toouse hello MQ-anslutningen i Azure Logic Apps | Microsoft Docs
description: "Ansluta tooan lokal eller Azure MQ-server från din logik app arbetsflöde toobrowse, ta emot och skicka meddelanden tooWebSphere MQ"
services: logic-apps
author: valthom
manager: anneta
documentationcenter: 
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 06/01/2017
ms.author: valthom; ladocs
ms.openlocfilehash: 8b36d53b457ced1a7461c229aecfcf8e4ae668a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-ibm-mq-server-from-logic-apps-using-hello-mq-connector"></a>Ansluta tooan IBM MQ-server från logikappar med hjälp av hello MQ connector 

Microsoft Connector för MQ skickar och tar emot meddelanden som lagras i ett MQ-Server lokalt eller i Azure. Denna koppling inkluderar en Microsoft MQ-klient som kommunicerar med en fjärransluten IBM MQ-server i ett TCP/IP-nätverk. Det här dokumentet är en starter guiden toouse hello MQ-koppling. Vi rekommenderar att du börjar genom att bläddra i ett enda meddelande i en kö och försök sedan hello andra åtgärder.    

hello MQ koppling inkluderar hello följande åtgärder. Det finns inga utlösare.

-   Bläddra i ett enda meddelande utan att ta bort hello-meddelande från hello IBM MQ-Server
-   Bläddra i en grupp med meddelanden utan att ta bort hälsningsmeddelande från hello IBM MQ-Server
-   Ett enda meddelande och ta bort hello-meddelande från hello IBM MQ-Server
-   Ta emot en grupp med meddelanden och ta bort hälsningsmeddelande från hello IBM MQ-Server
-   Skicka ett enda meddelande toohello IBM MQ-Server 

## <a name="prerequisites"></a>Krav

* Om du använder en lokal MQ-server [installera hello lokala datagateway](../logic-apps/logic-apps-gateway-install.md) på en server i nätverket. Om hello MQ-Server är offentligt tillgänglig, eller tillgängliga i Azure, sedan hello datagateway inte används eller krävs.

    > [!NOTE]
    > hello-server där hello lokala Data Gateway har installerats måste ha .net Framework 4.6 installerats för hello MQ Connector toofunction.

* Skapa hello Azure-resurs för hello lokala datagateway - [konfigurera hello data gateway-anslutningen](../logic-apps/logic-apps-gateway-connection.md).

* Officiellt IBM WebSphere MQ-versioner som stöds:
   * MQ 7.5
   * MQ 8.0

## <a name="create-a-logic-app"></a>Skapa en logikapp

1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**. 
2. Ange hello **namn**, till exempel MQTestApp, **prenumeration**, **resursgruppen**, och **plats** (använda hello plats där hello lokala Data Gateway-anslutningen har konfigurerats). Välj **PIN-kod toodashboard**, och välj **skapa**.  
![Skapa Logikapp](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Lägg till en utlösare

> [!NOTE]
> hello MQ Connector har inte utlösare. Så, Använd en annan utlösare toostart din logikapp, till exempel hello **återkommande** utlösare. 

1. Hej **Logic Apps Designer** öppnas väljer **återkommande** i hello lista över vanliga utlösare.
2. Välj **redigera** inom hello upprepning utlösare. 
3. Ange hello **frekvens** för**dag**, och ange hello **intervall** för**7**. 

## <a name="browse-a-single-message"></a>Bläddra i ett enda meddelande
1. Välj **+ nytt steg**, och välj **lägga till en åtgärd**.
2. Skriv i sökrutan hello `mq`, och välj sedan **MQ - Bläddra meddelandet**.  
![Bläddra meddelande](media/connectors-create-api-mq/Browse_message.png)

3. Om det inte finns en befintlig MQ-anslutning, skapar du hello-anslutningen:  

    1. Välj **Anslut via lokala datagateway**, och ange egenskaper för hello MQ-Server.  
    För **Server**, du kan ange hello MQ-servernamn eller ange hello IP-adressen följt av ett kolon och hello portnummer. 
    2. Hej **gateway** listan visar alla befintliga gateway-anslutningar som har konfigurerats. Välj din gateway.
    3. Välj **skapa** när du är klar. Anslutningen ser ut ungefär toohello följande:   
    ![Anslutningsegenskaper](media/connectors-create-api-mq/Connection_Properties.png)

4. I egenskaperna för hello-åtgärd kan du:  

    * Använd hello **kön** egenskapen tooaccess en annan kö än vad som har definierats i hello-anslutning
    * Använd hello **MessageId**, **CorrelationId**, **GroupId**, och andra egenskaper toobrowse för ett meddelande utifrån hello olika MQ meddelandeegenskaper
    * Ange **IncludeInfo** för**SANT** tooinclude ytterligare meddelandeinformation i hello utdata. Eller ändra värdet för**FALSKT** toonot inkludera information om ytterligare meddelande i hello utdata.
    * Ange en **Timeout** värdet toodetermine hur länge toowait för ett meddelande tooarrive i en tom kö. Om inget anges hello första meddelandet i kön hello hämtas och det finns ingen tid väntar på ett meddelande tooappear.  
    ![Bläddra meddelandeegenskaper](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Spara** dina ändringar och sedan **kör** logikappen:  
![Spara och köra](media/connectors-create-api-mq/Save_Run.png)

6. Hello stegen för att köra hello visas efter några sekunder och titta på hello utdata. Välj hello grön bockmarkering toosee information för varje steg. Välj **finns rådata utdata** toosee ytterligare information om hello utdata.  
![Bläddra meddelandet utdata](media/connectors-create-api-mq/Browse_message_output.png)  

    Rådata utdata:  
    ![Bläddra meddelandet raw-utdata](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. När hello **IncludeInfo** alternativuppsättning tootrue, hello följande utdata visas:  
![Bläddra meddelandet innehålla info](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Bläddra flera meddelanden
Hej **Bläddra meddelanden** åtgärden inkluderar en **BatchSize** alternativet tooindicate hur många meddelanden ska returneras från hello kö.  Om **BatchSize** har ingen post returneras alla meddelanden. hello returnerade utdata är en matris av meddelanden.

1. När du lägger till hello **Bläddra meddelanden** åtgärd hello första anslutningen har konfigurerats är markerad som standard. Välj **ändra anslutningen** toocreate en ny anslutning eller välj en annan anslutning.

2. hello utdata från Bläddra meddelanden visas:  
![Bläddra meddelanden utdata](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Ett enda meddelande
Hej **ta emot meddelandet** åtgärd har hello samma in- och utgångar som hello **Bläddra meddelandet** åtgärd. När du använder **ta emot meddelandet**, hello-meddelande tas bort från kön hello.

## <a name="receive-multiple-messages"></a>Ta emot flera meddelanden
Hej **ta emot meddelanden** åtgärd har hello samma in- och utgångar som hello **Bläddra meddelanden** åtgärd. När du använder **ta emot meddelanden**, hälsningsmeddelande tas bort från kön hello.

Om det finns inga meddelanden i kö för hello när du gör en Bläddra eller en receive, misslyckas hello steg med hello följande utdata:  
![MQ Nej visas fel](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>Skicka ett meddelande
1. När du lägger till hello **skickar ett meddelande** åtgärd hello första anslutningen har konfigurerats är markerad som standard. Välj **ändra anslutningen** toocreate en ny anslutning eller välj en annan anslutning. hello giltig **meddelandetyper** är **Datagram**, **svar**, eller **begära**.  
![Skicka meddelande sammanställer](media/connectors-create-api-mq/Send_Msg_Props.png)

2. hello utdata för Skicka meddelande ser ut så hello följande:  
![Skicka meddelande](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/mq/).

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).
