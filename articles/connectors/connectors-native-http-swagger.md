---
title: "aaaCall REST-slutpunkter med HTTP + Swagger-koppling för Logikappar i Azure | Microsoft Docs"
description: "Ansluta tooREST slutpunkter från logikappar via Swagger med hello HTTP + Swagger-koppling"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: baaa57689ff41fcd052f9d86086e36619ddec46e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http--swagger-action"></a>Kom igång med hello HTTP + Swagger-åtgärd

Du kan skapa en förstklassig connector tooany REST-slutpunkt via en [Swagger-dokument](https://swagger.io) när du använder hello HTTP + Swagger-åtgärd i logik app arbetsflödet. Du kan också utöka logic apps toocall valfri REST-slutpunkt förstklassigt logik App Designer upplevelse.

hur toocreate logikappar med kopplingar, se toolearn [skapa en ny logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Använd HTTP + Swagger som en utlösare eller en åtgärd

hello HTTP + Swagger-trigger och action arbete hello samma som hello [HTTP-åtgärd](connectors-native-http.md) men ge en bättre upplevelse i logik App Designer genom att exponera hello API struktur och utdata från hello [Swagger-metadata](https://swagger.io) . Du kan också använda hello HTTP + Swagger-koppling som en utlösare. Om du vill tooimplement en avsökning utlösare följer hello avsökning mönster som beskrivs i [skapa anpassade API: er toocall andra API: er, tjänster och system från logikappar](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

Lär dig mer om [logik app utlösare och åtgärder](connectors-overview.md).

Här är ett exempel på hur toouse hello HTTP + Swagger igen som en åtgärd i ett arbetsflöde i en logikapp.

1. Välj hello **nytt steg** knappen.
2. Välj **lägga till en åtgärd**.
3. Skriv i sökrutan för hello åtgärd **swagger** toolist hello HTTP + Swagger-åtgärd.
   
    ![Välj HTTP + Swagger åtgärd](./media/connectors-native-http-swagger/using-action-1.png)
4. Ange hello URL för ett Swagger-dokument:
   
   * toowork från hello logik App Designer hello URL måste vara en HTTPS-slutpunkt och har CORS aktiverat.
   * Om hello Swagger-dokument inte uppfylla detta krav kan du använda [Azure Storage med CORS aktiverat](#hosting-swagger-from-storage) toostore hello dokumentet.
5. Klicka på **nästa** tooread och visa från hello Swagger-dokument.
6. Lägga till i alla parametrar som krävs för hello HTTP-anrop.
   
    ![Slutföra HTTP-åtgärden](./media/connectors-native-http-swagger/using-action-2.png)
7. toosave och publicera logikappen klickar du på **spara** designer i verktygsfältet.

### <a name="host-swagger-from-azure-storage"></a>Värden Swagger från Azure Storage
Du kanske vill tooreference Swagger-dokument som inte finns eller som inte uppfyller kraven för hello säkerhet och cross-origin för hello designer. tooresolve det här problemet kan du lagra hello Swagger-dokument i Azure Storage och aktivera CORS tooreference hello dokumentet.  

Här följer hello steg toocreate, konfigurera och lagra Swagger-dokument i Azure Storage:

1. [Skapa ett Azure storage-konto med Azure Blob storage](../storage/common/storage-create-storage-account.md). tooperform detta steg kan ange behörigheter för**offentlig åtkomst**.

2. Aktivera CORS för hello-blob. 

   tooautomatically konfigurerar den här inställningen kan du använda [detta PowerShell-skript](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

3. Överför hello Swagger-filen toohello blob. 

   Du kan utföra det här steget från hello [Azure-portalen](https://portal.azure.com) eller från ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/).

4. Referera till ett HTTPS länken toohello dokument i Azure Blob storage. 

   hello länk använder det här formatet:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Teknisk information
Följande är hello information för hello utlösare och åtgärder som den här HTTP + Swagger anslutningen stöder.

## <a name="http--swagger-triggers"></a>HTTP- + Swagger-utlösare
En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp. [Mer information om utlösare.](connectors-overview.md) Hej HTTP + Swagger koppling har en utlösare.

| Utlösare | Beskrivning |
| --- | --- |
| HTTP- + Swagger |Gör ett HTTP-anrop och returnerar hello svar innehåll |

## <a name="http--swagger-actions"></a>HTTP- + Swagger-åtgärder
En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp. [Mer information om åtgärder.](connectors-overview.md) Hej HTTP + Swagger koppling har en möjlig åtgärd.

| Åtgärd | Beskrivning |
| --- | --- |
| HTTP- + Swagger |Gör ett HTTP-anrop och returnerar hello svar innehåll |

### <a name="action-details"></a>Åtgärdsinformation
Hej HTTP + Swagger connector medföljer en möjlig åtgärd. Följande är information om varje hello åtgärder, obligatoriska och valfria inmatningsfält och hello motsvarande utdata information som är associerade med deras användning.

#### <a name="http--swagger"></a>HTTP- + Swagger
Göra en utgående HTTP-begäran med hjälp av Swagger-metadata.
En asterisk (*) innebär ett obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Metoden * |Metoden |HTTP-verb toouse. |
| URI * |URI: N |URI för hello HTTP-begäran. |
| Rubriker |Rubriker |Ett JSON-objekt i HTTP-huvuden tooinclude. |
| Innehåll |Brödtext |hello text för HTTP-begäran. |
| Autentisering |Autentisering |Autentisering toouse för begäran. Mer information finns i hello [HTTP-anslutningen](connectors-native-http.md#authentication). |

**Information för utdata**

HTTP-svar

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Rubriker |Objektet |Svarsrubriker |
| Innehåll |Objektet |Objektet Response |
| Statuskod |int |HTTP-statuskod |

### <a name="http-responses"></a>HTTP-svar
När du gör anrop toovarious åtgärder kan få du vissa svar. Följande är en tabell som visar motsvarande svar och beskrivningar.

| Namn | Beskrivning |
| --- | --- |
| 200 |OKEJ |
| 202 |Godkänt |
| 400 |Felaktig begäran |
| 401 |Behörighet saknas |
| 403 |Tillåts inte |
| 404 |Det gick inte att hitta |
| 500 |Internt serverfel. Okänt fel uppstod. |

- - -
## <a name="next-steps"></a>Nästa steg

* [Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md)
* [Sök efter andra kopplingar](apis-list.md)