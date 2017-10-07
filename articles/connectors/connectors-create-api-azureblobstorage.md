---
title: aaaAdd hello Azure blob storage-anslutningen i dina Logic Apps | Microsoft Docs
description: "Komma igång och konfigurera hello Azure blob storage-anslutningen i en logikapp"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: add61287ef1b2228ef9d3f54ce082807bad6858b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-blob-storage-connector-in-a-logic-app"></a>Använd hello Azure blob storage-anslutningen i en logikapp
Använd hello Azure Blob storage connector tooupload, uppdatera, hämta och ta bort blobbar i ditt lagringskonto alla inom en logikapp.  

Med Azure blob storage kan du:

* Skapa ditt arbetsflöde genom att ladda upp nya projekt eller hämta filer som nyligen har uppdaterats.
* Använda åtgärder tooget filmetadata, ta bort en fil och kopiera filer. Till exempel när ett verktyg uppdateras i en Azure-webbplats (en utlösare), sedan uppdatera en fil i blob storage (en åtgärd). 

Det här avsnittet visar hur toouse hello blob storage-kopplingen i en logikapp.

toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooazure-blob-storage"></a>Ansluta tooAzure blob-lagring
Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service. En anslutning kan du ansluta en logikapp och en annan tjänst. Till exempel tooconnect tooa storage-konto du först skapa en blob-lagring *anslutning*. toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess hello-tjänsten som du ansluter till. Ange så hello autentiseringsuppgifter tooyour storage-konto toocreate hello anslutning med Azure storage. 

#### <a name="create-hello-connection"></a>Skapa hello-anslutning
> [!INCLUDE [Create a connection tooAzure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a>Använda en utlösare
Den här anslutningen har inte utlösare. Använda andra utlösare toostart hello logikapp, till exempel en upprepning utlösare, en HTTP-Webhook-utlösare, utlösare som är tillgängliga med övriga kopplingar och mycket mer. [Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) innehåller ett exempel.

## <a name="use-an-action"></a>Använda en åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.

1. Välj hello plustecken. Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Välj **lägga till en åtgärd**.
3. Skriv ”blob” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. I vårt exempel väljer **AzureBlob - Get filens metadata med sökvägen**. Om det finns redan en anslutning, och välj sedan hello **...** (Visa Väljaren) knappen tooselect en fil.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning. [Skapa hello anslutning](connectors-create-api-azureblobstorage.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper. 
   
   > [!NOTE]
   > I det här exemplet kan vi hämta hello metadata för en fil. toosee hello metadata, Lägg till en annan åtgärd som skapar en ny fil med en annan koppling. Till exempel lägga till en OneDrive-åtgärd som skapar en ny ”test” fil baserat på hello metadata. 


5. **Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet). Din logikapp sparas och aktiveras automatiskt.

> [!TIP]
> [Lagringsutforskaren](http://storageexplorer.com/) är ett bra verktyg för att hantera flera lagringskonton.

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/azureblobconnector/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).

