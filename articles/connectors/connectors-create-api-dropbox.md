---
title: aaaDropbox connector i Azure Logic Apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. Ansluta tooDropbox toomanage dina filer. Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i Dropbox."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1f307477836104c0bc0008341604a1400860987f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-dropbox-connector"></a>Kom igång med hello Dropbox-koppling
Ansluta tooDropbox toomanage dina filer. Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i Dropbox.

toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp. Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toodropbox"></a>Ansluta tooDropbox
Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service. En anslutning kan du ansluta en logikapp och en annan tjänst. Till exempel i ordning tooconnect tooDropbox måste du först en Dropbox *anslutning*. toocreate en anslutning måste tooprovide hello autentiseringsuppgifter som du vanligtvis använder tooaccess Hej tjänst du vill tooconnect till. Så i hello Dropbox exempelvis behöver du hello autentiseringsuppgifter tooyour Dropbox-konto i ordning toocreate hello anslutning tooDropbox. [Mer information om anslutningar]()

### <a name="create-a-connection-toodropbox"></a>Skapa en anslutning tooDropbox
> [!INCLUDE [Steps toocreate a connection tooDropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Använda en Dropbox-utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

I det här exemplet ska vi använda hello **när en fil skapas** utlösare. När den här utlösaren uppstår kan vi ringer hello **hämta innehåll med sökvägen** Dropbox-åtgärd. 

1. Ange *dropbox* i sökrutan på hello Logic Apps designer hello sedan väljer hello **Dropbox - när en fil skapas** utlösare.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Välj hello mapp där du vill skapa tootrack. Välj... (som identifieras i hello röd ruta) och bläddra toohello mapp som du vill tooselect för hello utlösare input.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Använda en Dropbox-åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Nu när hello utlösaren har lagts till, följa dessa steg tooadd en åtgärd som kommer att få hello nya dess innehåll.

1. Välj **+ nytt steg** tooadd hello åtgärd som tootake när en ny fil skapas.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Välj **lägga till en åtgärd**. Den här öppnas hello-sökrutan där du kan söka efter något du vill tootake.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Ange *dropbox* toosearch för åtgärder relaterade tooDropbox.  
4. Välj **Dropbox - Get-filinnehåll med sökvägen** hello åtgärd tootake när en ny fil skapas i hello markerad Dropbox-mappen. hello åtgärd kontrollen block öppnas. Du kommer att tillfrågas tooauthorize din logik app tooaccess din Dropbox-konto om du inte har gjort det tidigare.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Välj... (på hello höger sida av hello **filsökväg** kontroll) och bläddra toohello sökväg som toouse. Eller Använd hello **filsökväg** token toospeed in genereringen av din app logik.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Spara ditt arbete och skapa en ny fil i Dropbox tooactivate arbetsflödet.  

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/dropbox/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka toohello [API: er listan](apis-list.md).
