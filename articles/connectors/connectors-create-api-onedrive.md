---
title: aaaAdd hello OneDrive connector i dina Logic Apps | Microsoft Docs
description: "Översikt över hello OneDrive koppling med REST API-parametrar"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 8303794bb3c2844de288f87f40639abb84c160fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-onedrive-connector"></a>Kom igång med hello OneDrive-koppling
Ansluta tooOneDrive toomanage filerna, inklusive överföring, get, ta bort filer och mycket mer. 

Med OneDrive kan du: 

* Skapa ditt arbetsflöde genom att lagra filer i OneDrive eller uppdatera befintliga filer i OneDrive. 
* Använd utlösare toostart arbetsflödet när en fil skapas eller uppdateras i OneDrive.
* Använda åtgärder toocreate en fil, ta bort en fil med mera. Till exempel när en ny Office 365 e-post tas emot med en bifogad fil (en utlösare), skapa en ny fil i OneDrive (en åtgärd).

Det här avsnittet visar hur toouse hello OneDrive kopplingen i en logikapp och även visar hello utlösare och åtgärder.

toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooonedrive"></a>Ansluta tooOneDrive
Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service. En anslutning kan du ansluta en logikapp och en annan tjänst. Till exempel tooconnect tooOneDrive, måste du först en OneDrive *anslutning*. toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess Hej tjänst du vill tooconnect till. Så, OneDrive, ange hello autentiseringsuppgifter tooyour OneDrive-konto toocreate hello anslutning.

### <a name="create-hello-connection"></a>Skapa hello-anslutning
> [!INCLUDE [Steps toocreate a connection tooOneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Använda en utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. Utlösare avsöker ”” hello-tjänsten på ett intervall och frekvens som du vill. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Skriv ”onedrive” tooget en lista över hello utlösare i hello logikapp:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Välj **när en fil ändras**. Om det finns redan en anslutning, väljer du hello visa Väljaren knappen tooselect en mapp.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Om du tillfrågas toosign i kan sedan ange hello inloggningsnamnet information toocreate hello anslutning. [Skapa hello anslutning](connectors-create-api-onedrive.md#create-the-connection) i det här avsnittet listar hello steg. 
   
   > [!NOTE]
   > I det här exemplet hello logikapp körs när en fil i hello-mapp som du väljer uppdateras. toosee hello resultaten av den här utlösaren Lägg till en annan åtgärd som skickar ett e-postmeddelande. Lägg exempelvis till hello Office 365 Outlook *skickar ett e-* åtgärd som e-postmeddelanden när en fil har uppdaterats. 

3. Välj hello **redigera** knappen och ange hello **frekvens** och **intervall** värden. Till exempel om du vill hello utlösaren toopoll var 15: e minut, ange hello **frekvens** för**minut**, och ange hello **intervall** för**15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet). Din logikapp sparas och aktiveras automatiskt.

## <a name="use-an-action"></a>Använda en åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Välj hello plustecken. Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Välj **lägga till en åtgärd**.
3. Skriv ”onedrive” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. I vårt exempel väljer **OneDrive – skapa filen**. Om det finns redan en anslutning, och välj sedan hello **mappsökväg** tooput hello fil, ange hello **filnamn**, och välj hello **filinnehåll** du vill:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning. [Skapa hello anslutning](connectors-create-api-onedrive.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper. 
   
   > [!NOTE]
   > I det här exemplet skapar vi en ny fil i en mapp i OneDrive. Du kan använda utdata från en annan utlösare toocreate hello OneDrive fil. Lägg exempelvis till hello Office 365 Outlook *när en ny e-postmeddelandet* utlösare. Lägg sedan till hello OneDrive *skapa fil* åtgärd som använder hello bilagor och Content-Type-fält i en ForEach toocreate hello ny fil i OneDrive. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet). Din logikapp sparas och aktiveras automatiskt.


## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka toohello [API: er listan](apis-list.md).
