---
title: aaaSMTP connector i Azure Logic Apps | Microsoft Docs
description: Skapa logikappar med Azure App service. Ansluta tooSMTP toosend e-post.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 36bb836851014d24f2e069fda8376ad7a08c943b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-smtp-connector"></a>Kom igång med hello SMTP-anslutaren
Ansluta tooSMTP toosend e-post.

toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp. Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toosmtp"></a>Ansluta tooSMTP
Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service. En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst. Till exempel tooconnect tooSMTP, måste du först en SMTP *anslutning*. toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess hello-tjänsten som du ansluter till. Så i hello SMTP exemplet anger du hello autentiseringsuppgifter tooyour anslutningens namn, SMTP-serveradressen och användaren logga in information toocreate hello anslutning tooSMTP.  

### <a name="create-a-connection-toosmtp"></a>Skapa en anslutning tooSMTP
> [!INCLUDE [Steps toocreate a connection tooSMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Använda en SMTP-utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

I det här exemplet SMTP har inte en utlösare egna, använder vi hello **Salesforce - när ett objekt skapas** utlösare. Den här utlösaren aktiveras när ett nytt objekt skapas i Salesforce. I vårt exempel vi ska konfigurera den så att varje gång en ny lead skapas i Salesforce, en *skicka e-post* sker via hello SMTP-koppling med ett meddelande om hello ny lead håller på att skapas.

1. Ange *salesforce* i hello sökrutan på hello logic apps designer väljer du sedan hello **Salesforce - när ett objekt skapas** utlösare.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. Hej **när ett objekt skapas** kontrollen visas.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. Välj hello **objekttyp** Välj *leda* hello listan över objekt. I det här steget anger du att du skapar en utlösare som meddelar logikappen när en ny lead skapas i Salesforce.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. hello utlösaren har skapats.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Använda en SMTP-åtgärden
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Nu när hello utlösaren har lagts till, följa dessa steg tooadd en SMTP-åtgärden som utförs när en ny lead skapas i Salesforce.

1. Välj **+ nytt steg** tooadd hello åtgärd som tootake när en ny lead skapas.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. Välj **lägga till en åtgärd**. Den här öppnas hello-sökrutan där du kan söka efter något du vill tootake.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. Ange *smtp* toosearch för åtgärder relaterade tooSMTP.  
4. Välj **SMTP - skicka e-post** som hello åtgärd tootake när hello ny lead skapas. hello åtgärd kontrollen block öppnas. Du har tooestablish SMTP-anslutningen i hello designer block om du inte har gjort det tidigare.  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. Ange önskad e-information på hello **SMTP - skicka e-post** block.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. Spara ditt arbete i ordning tooactivate arbetsflödet.  

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/smtpconnector/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka toohello [API: er listan](apis-list.md).
