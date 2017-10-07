---
title: "aaaIoT fjärrövervaknings och meddelanden med Azure Logikappar | Microsoft Docs"
description: "Använd Azure Logikappar för övervakning av IoT temperatur på din IoT-hubb och automatiskt skicka e-postaviseringar tooyour postlåda efter eventuella avvikelser som upptäckts."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT övervakningsaviseringar, iot, iot temperaturövervakning"
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 89396528ed63c37258e1b49f342f0723e686ecb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IoT fjärrövervaknings och meddelanden med Azure Logikappar ansluta din IoT-hubb och postlåda

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Med Azure Logikappar kan tooautomate processer som en serie steg. En logikapp kan ansluta över olika tjänster och protokoll. Den börjar med en utlösare som ”när ett konto har lagts till”, och följas av en kombination av åtgärder, så som 'Skicka ett push-meddelande'. Den här funktionen gör Logic Apps en perfekt IoT-lösning för IoT övervakning, till exempel används avisering efter avvikelser bland andra Användningsscenarier.

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig hur toocreate en logikapp som ansluter din IoT-hubb och postlådan för temperaturövervakning och aviseringar. När hello temperaturen är över 30 C, hello klienten program markerar `temperatureAlert = "true"` i hello-meddelande skickas tooyour IoT-hubb. hello-meddelande utlösare hello logik app toosend du ett e-postmeddelande.

## <a name="what-you-do"></a>Vad du gör

* Skapa ett namnområde för service bus och Lägg till en kö tooit.
* Lägga till en slutpunkt och en routning regeln tooyour IoT-hubb.
* Skapa, konfigurera och testa en logikapp.

## <a name="what-you-need"></a>Vad du behöver

* Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:
  * En aktiv Azure-prenumeration.
  * En Azure IoT-hubb i din prenumeration.
  * Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.

## <a name="create-service-bus-namespace-and-add-a-queue-tooit"></a>Skapa service bus-namnrymd och lägga till en kö tooit

### <a name="create-a-service-bus-namespace"></a>Skapa ett namnområde för service bus

1. På hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Enterprise Integration** > **Service Bus**.
1. Ange hello följande information:

   **Namnet**: hello namn i hello service bus.

   **Prisnivån**: Klicka på **grundläggande** > **Välj**. hello grundnivån är tillräcklig för den här kursen.

   **Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.

   **Plats**: Använd hello samma plats som använder din IoT-hubb.
1. Klicka på **Skapa**.

   ![Skapa en service bus-namnrymd i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Lägg till en service bus-kö

1. Öppna hello service bus-namnrymd och klicka sedan på **+ kö**.
1. Ange ett namn för hello kön och klicka sedan på **skapa**.
1. Öppna hello service bus-kö och klicka sedan på **principer för delad åtkomst** > **+ Lägg till**.
1. Ange ett namn för principen för hello, kontrollera **hantera**, och klicka sedan på **skapa**.

   ![Lägg till en service bus-kö i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-tooyour-iot-hub"></a>Lägg till en slutpunkt och en routning regeln tooyour IoT-hubb

### <a name="add-an-endpoint"></a>Lägga till en slutpunkt

1. Öppna din IoT-hubb, klicka på slutpunkter > + Lägg till.
1. Ange hello följande information:

   **Namnet**: hello namnet på hello slutpunkt.

   **Slutpunktstypen**: Välj **Service Bus-kö**.

   **Service Bus-namnrymd**: Välj hello namnområdet som du skapade.

   **Service Bus-kö**: Välj hello kön som du skapade.
1. Klicka på **OK**.

   ![Lägga till en slutpunkt tooyour IoT-hubb i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Lägg till en regel för vidarebefordran

1. Klicka på din IoT-hubb **vägar** > **+ Lägg till**.
1. Ange hello följande information:

   **Namnet**: hello namnet på regel för vidarebefordran av hello.

   **Datakällan**: Välj **DeviceMessages**.

   **Slutpunkt**: Välj hello-slutpunkt som du skapade.

   **Frågesträng**: Ange `temperatureAlert = "true"`.
1. Klicka på **Spara**.

   ![Lägga till en regel för vidarebefordran i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Skapa och konfigurera en logikapp

### <a name="create-a-logic-app"></a>Skapa en logikapp

1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Enterprise Integration** > **Logikapp**.
1. Ange hello följande information:

   **Namnet**: hello namnet på hello logikapp.

   **Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.

   **Plats**: Använd hello samma plats som använder din IoT-hubb.
1. Klicka på **Skapa**.

### <a name="configure-hello-logic-app"></a>Konfigurera hello logikapp

1. Öppna hello logikappen som öppnas i hello Logic Apps Designer.
1. I hello Logic Apps Designer, klickar du på **tom Logikapp**.

   ![Börja med en tom logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Klicka på **Service Bus**.

   ![Välj Service Bus toostart skapa din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Klicka på **Service Bus – när en eller flera meddelanden tas emot i en kö (automatisk komplettering)**.
1. Skapa en service bus-anslutning.
   1. Ange ett anslutningsnamn.
   1. Klicka på hello service bus-namnrymd > Hej service bus-policy > **skapa**.

      ![Skapa en service bus-anslutning för din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Klicka på **Fortsätt** när hello service bus-anslutning har skapats.
   1. Välj hello kön som du skapade och ange `175` för **maximalt antal för meddelande**

      ![Ange hello högsta tillåtna antal för hello service bus-anslutning i din logikapp](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Klicka på ”Spara” knappen toosave hello ändras.

1. Skapa en SMTP-tjänst-anslutningen.
   1. Klicka på **nytt steg** > **lägga till en åtgärd**.
   1. Typen `SMTP`, klicka på hello **SMTP** -tjänsten i hello sökresultatet och klicka sedan på **SMTP - skicka e-post**.

      ![Skapa en SMTP-anslutning i din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Ange hello SMTP-information för din postlåda och klicka sedan på **skapa**.

      ![Ange SMTP-anslutningsinformation i din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Hämta hello SMTP-information för [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), och [Yahoo e-post](https://help.yahoo.com/kb/SLN4075.html).
   1. Ange din e-postadress för **från** och **till**, och `High temperature detected` för **ämne** och **brödtext**.
   1. Klicka på **Spara**.

Hej logikapp är fungerar när du sparar den.

## <a name="test-hello-logic-app"></a>Testa hello logikapp

1. Starta hello klientprogram som du distribuerar tooyour enhet i [ansluta ESP8266 tooAzure IoT-hubb](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. Hello miljö temperatur runt hello SensorTag toobe över 30 C. Till exempel ljus en stearinljusstapel runt din SensorTag.
1. Du bör få ett e-postmeddelande som skickats av hello logikapp.

   > [!NOTE]
   > Din e-post-leverantör behöva tooverify hello avsändaren identitet toomake är du som skickar hello e-post.

## <a name="next-steps"></a>Nästa steg

Du har skapat en logikapp som ansluter din IoT-hubb och postlådan för temperaturövervakning och aviseringar.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
