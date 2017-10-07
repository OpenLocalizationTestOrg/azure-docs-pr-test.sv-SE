---
title: aaaAdd hello Office 365 Outlook connector i dina Logic Apps | Microsoft Docs
description: 'Skapa logikappar med Office 365 connector tooenable interaktion med Office 365. Exempel: skapa, redigera och uppdatera kontakter och Kalender-objekt.'
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 86a573c9c54701de3d3f0500d19eaf545e0710ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-office-365-outlook-connector"></a>Kom igång med hello Office 365 Outlook connector
hello Office 365 Outlook connector kan interaktion med Outlook i Office 365. Använd den här kopplingen toocreate, redigera, och uppdatera kontakter och kalenderobjekt, och även hämta, skicka och svara tooemail.

Med Office 365 Outlook kan du:

* Skapa arbetsflödet använder hello e-post och kalender funktioner i Office 365. 
* Använd utlöser toostart arbetsflödet när det finns ett nytt e-postmeddelande när ett kalenderobjekt uppdateras med mera.
* Använd åtgärder toosend ett e-postmeddelande, skapa en ny händelse och mycket mer. Till exempel när det finns ett nytt objekt i Salesforce (en utlösare), skicka ett e-postmeddelande tooyour Office 365 Outlook (en åtgärd). 

Det här avsnittet visar hur toouse hello Outlook för Office 365-kopplingen i en logikapp och även visar hello utlösare och åtgärder.

> [!NOTE]
> Den här versionen av hello artikeln gäller tooLogic appar allmän tillgänglighet (GA).
> 
> 

toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toooffice-365"></a>Ansluta tooOffice 365
Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service. En anslutning kan du ansluta en logikapp och en annan tjänst. Till exempel tooconnect tooOffice 365 Outlook, måste du först en Office 365 *anslutning*. toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess Hej tjänst du vill tooconnect till. Ange så hello autentiseringsuppgifter tooyour Office 365-konto toocreate hello anslutning med Office 365 Outlook.

## <a name="create-hello-connection"></a>Skapa hello-anslutning
> [!INCLUDE [Steps toocreate a connection tooOffice 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Använda en utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. Utlösare avsöker ”” hello-tjänsten på ett intervall och frekvens som du vill. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Skriv ”office 365” tooget en lista över hello utlösare i hello logikapp:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Välj **Office 365 Outlook - när en kommande händelse startas snart**. Om det finns redan en anslutning, väljer du en kalender hello nedrullningsbara listan.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Om du tillfrågas toosign i kan sedan ange hello inloggningsnamnet information toocreate hello anslutning. [Skapa hello anslutning](connectors-create-api-office365-outlook.md#create-the-connection) i det här avsnittet listar hello steg. 
   
   > [!NOTE]
   > I det här exemplet hello logikapp körs när en händelse har uppdaterats. toosee hello resultaten av den här utlösaren Lägg till en annan åtgärd som skickar ett SMS. Lägg exempelvis till hello Twilio *skickar ett meddelande* åtgärd som texter när hello kalender händelse startar i 15 minuter. 
   > 
   > 
3. Välj hello **redigera** knappen och ange hello **frekvens** och **intervall** värden. Till exempel om du vill hello utlösaren toopoll var 15: e minut, ange hello **frekvens** för**minut**, och ange hello **intervall** för**15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet). Din logikapp sparas och aktiveras automatiskt.

## <a name="use-an-action"></a>Använda en åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Välj hello plustecken. Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Välj **lägga till en åtgärd**.
3. Skriv ”office 365” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. I vårt exempel väljer **Office 365 Outlook - skapa kontakt**. Om det finns redan en anslutning, och välj sedan hello **mappen ID**, **Förnamn**, och andra egenskaper:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning. [Skapa hello anslutning](connectors-create-api-office365-outlook.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper. 
   
   > [!NOTE]
   > I det här exemplet skapar vi en ny kontakt i Office 365 Outlook. Du kan använda utdata från en annan utlösare toocreate hello kontakt. Lägg exempelvis till hello SalesForce *när ett objekt skapas* utlösare. Lägg sedan till hello Office 365 Outlook *skapa kontakt* åtgärd som använder hello SalesForce fält toocreate hello ny nya kontakt i Office 365. 
   > 
   > 
5. **Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet). Din logikapp sparas och aktiveras automatiskt.

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/office365connector/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).

