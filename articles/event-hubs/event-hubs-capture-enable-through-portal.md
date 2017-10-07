---
title: aaaAzure Event Hubs avbilda aktivera via portalen | Microsoft Docs
description: "Aktivera hello Event Hubs Capture-funktionen med hjälp av hello Azure-portalen."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 27c7528552c497a4d98873a22bd56a991c66247c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-event-hubs-capture-using-hello-azure-portal"></a>Aktivera Event Hubs hämtar med hello Azure-portalen

Du kan konfigurera avbilda vid hello event hub skapandet med hello [Azure-portalen](https://portal.azure.com). Du kan antingen avbildning hello data tooan Azure [Blob storage](https://azure.microsoft.com/services/storage/blobs/) behållare eller tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) konto.

## <a name="capture-data-tooan-azure-storage-account"></a>Samla in data tooan Azure Storage-konto  

När du skapar en händelsehubb kan du aktivera avbildning genom att klicka på hello **på** knapp i hello **skapa Event Hub** portalblad. Sedan anger du ett Lagringskonto och behållare genom att klicka på **Azure Storage** i hello **avbilda providern** rutan. Eftersom Event Hubs avbilda använder tjänst-till-tjänst-autentisering med lagring, behöver inte toospecify anslutningssträngen för lagring. hello resurs Väljaren väljs automatiskt hello resurs-URI för ditt lagringskonto. Om du använder Azure Resource Manager måste du ange den här URI:n uttryckligen som en sträng.

hello standard tidsfönstret är 5 minuter. hello lägsta värdet är 1, hello maximala 15. Hej **storlek** fönstret har en uppsättning 10 500 MB.

![][1]

## <a name="capture-data-tooan-azure-data-lake-store-account"></a>Samla in data tooan Azure Data Lake Store-konto

toocapture data tooAzure Datasjölager du skapa ett Data Lake Store-konto och en händelsehubb:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Skapa ett Azure Data Lake Store-konto och mappar

1. Skapa ett Data Lake Store-konto hello instruktionerna i [Kom igång med Azure Data Lake Store med hjälp av hello Azure-portalen](../data-lake-store/data-lake-store-get-started-portal.md). 
2. Skapa en mapp under det här kontot hello instruktionerna i hello [skapa mappar i Azure Data Lake Store-konto](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) avsnitt.
3. I hello Data Lake Store-kontoblad klickar du på **Data Explorer**.
4. Klicka på **Åtkomst**.
5. Klicka på **Lägg till**.
6. I hello **Sök efter namn eller e-** skriver **Microsoft.EventHubs** och välj sedan det här alternativet. 
7. Hej **behörigheter** visas. Ange hello behörigheter som visas i följande bild hello:

    ![][6]

8. Klicka på **OK**.
9. Nu skapa en mapp i rotmappen för hello genom att bläddra toohello målmappen och klicka på hello mappnamn.
10. Klicka på **Åtkomst**.
11. Klicka på **Lägg till**.
12. I hello **Sök efter namn eller e-** skriver **Microsoft.EventHubs** och välj sedan det här alternativet.
13. Hej **behörigheter** visas igen. Ange hello behörigheter som visas i följande bild hello:

    ![][5]

### <a name="create-an-event-hub"></a>Skapa en händelsehubb

1. Observera att hello händelsehubb måste vara i hello samma Azure-prenumeration som hello Azure Data Lake Store som du nyss skapade. Skapa hello händelsehubb, klicka på hello **på** knappen **avbilda** i hello **skapa Event Hub** portalblad. 
2. I hello **skapa Event Hub** portalbladet och välj **Azure Data Lake Store** från hello **avbilda providern** rutan.
3. I **Välj Data Lake Store**, ange hello Data Lake Store-konto som du skapade tidigare och i hello **Data Lake sökvägen** anger hello sökvägen toohello datamapp som du skapade.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>Lägga till eller konfigurera avbildningsfunktionen i en befintlig händelsehubb

Du kan konfigurera avbildningsfunktionen i befintliga händelsehubbar som finns i namnområden för Event Hubs. tooenable på ett befintligt händelsehubb eller toochange avbilda dina hämtningsinställningar klickar du på hello namnområde tooload hello **Essentials** bladet Klicka hello händelsehubb som du vill tooenable eller ändra inställningen för hello avbildning. Klicka slutligen på hello **egenskaper** avsnitt i hello öppna bladet och redigera hello hämtningsinställningar enligt hello följande uppgifter:

### <a name="azure-blob-storage"></a>Azure Blob Storage

![][2]

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a>Nästa steg

Du kan också konfigurera avbildningsfunktionen i Event Hubs med hjälp av Azure Resource Manager-mallar. Mer information finns i [Skapa ett namnområde för Event Hubs med en händelsehubb och aktivera avbildning med hjälp av en Azure Resource Manager-mall](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).
