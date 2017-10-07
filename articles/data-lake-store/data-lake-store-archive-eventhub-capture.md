---
title: "aaaCapture data från Händelsehubbar i Azure Data Lake Store | Microsoft Docs"
description: "Använd Azure Data Lake Store toocapture data från Händelsehubbar"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 09b17bd0b47043bd2c83dba72c01a8064f206a0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-store-toocapture-data-from-event-hubs"></a>Använd Azure Data Lake Store toocapture data från Händelsehubbar

Lär dig hur toouse Azure Data Lake Store toocapture data tas emot av Händelsehubbar i Azure.

## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md).

*  **Ett namnområde för Händelsehubbar**. Instruktioner finns i [skapa ett namnområde för Händelsehubbar](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Kontrollera hello Data Lake Store-konto och hello Händelsehubbar namnområde i hello samma Azure-prenumeration.


## <a name="assign-permissions-tooevent-hubs"></a>Tilldela behörigheter tooEvent hubbar

I det här avsnittet skapar du en mapp i hello konto där du vill toocapture hello data från Händelsehubbar. Du också tilldela behörigheter tooEvent Hubs så att det kan skriva data till ett Data Lake Store-konto. 

1. Öppna hello Data Lake Store-konto där du vill toocapture data från Händelsehubbar och klicka sedan på **Data Explorer**.

    ![Data Lake Store data explorer](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Datasjölager data explorer")

2.  Klicka på **ny mapp** och ange sedan ett namn på mappen där du vill toocapture hello data.

    ![Skapa en ny mapp i Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "skapa en ny mapp i Data Lake Store")

3. Tilldela behörigheter hello roten av hello Data Lake Store. 

    a. Klicka på **Data Explorer**, Välj hello roten för hello Data Lake Store-konto och klicka sedan på **åtkomst**.

    ![Tilldela behörigheter för Data Lake Store roten](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "tilldela behörigheter för Data Lake Store-rot")

    b. Under **åtkomst**, klickar du på **Lägg till**, klickar du på **Välj användare eller grupp**, och sök sedan efter `Microsoft.EventHubs`. 

    ![Tilldela behörigheter för Data Lake Store roten](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "tilldela behörigheter för Data Lake Store-rot")
    
    Klicka på **Välj**.

    c. Under **tilldela behörigheter**, klickar du på **Välj behörigheter**. Ange **behörigheter** för**kör**. Ange **lägga till** för**den här mappen och alla underordnade**. Ange **lägga till som** för**en behörighetspost för åtkomst och en standard behörighetspost**.

    ![Tilldela behörigheter för Data Lake Store roten](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "tilldela behörigheter för Data Lake Store-rot")

    Klicka på **OK**.

4. Tilldela behörigheter för hello mapp under Data Lake Store-konto där du vill toocapture data.

    a. Klicka på **Data Explorer**, Välj hello mapp i hello Data Lake Store-konto och klicka sedan på **åtkomst**.

    ![Tilldela behörigheter för Data Lake Store-mapp](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "tilldela behörigheter för Data Lake Store-mapp")

    b. Under **åtkomst**, klickar du på **Lägg till**, klickar du på **Välj användare eller grupp**, och sök sedan efter `Microsoft.EventHubs`. 

    ![Tilldela behörigheter för Data Lake Store-mapp](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "tilldela behörigheter för Data Lake Store-mapp")
    
    Klicka på **Välj**.

    c. Under **tilldela behörigheter**, klickar du på **Välj behörigheter**. Ange **behörigheter** för**läsa, skriva,** och **kör**. Ange **lägga till** för**den här mappen och alla underordnade**. Slutligen, Ställ **lägga till som** för**en behörighetspost för åtkomst och en standard behörighetspost**.

    ![Tilldela behörigheter för Data Lake Store-mapp](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "tilldela behörigheter för Data Lake Store-mapp")
    
    Klicka på **OK**. 

## <a name="configure-event-hubs-toocapture-data-toodata-lake-store"></a>Konfigurera Händelsehubbar toocapture tooData Datasjölager

I det här avsnittet skapar du en Händelsehubb i ett namnområde för Händelsehubbar. Du kan också konfigurera hello Event Hub toocapture data tooan Azure Data Lake Store-konto. Det här avsnittet förutsätter att du redan har skapat ett namnområde för Händelsehubbar.

2. Från hello **översikt** i hello Händelsehubbar namnområde, klickar du på **+ Event Hub**.

    ![Skapa Händelsehubb](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "skapar Händelsehubb")

3. Ange hello följande värden tooconfigure Händelsehubbar toocapture tooData Datasjölager.

    ![Skapa Händelsehubb](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "skapar Händelsehubb")

    a. Ange ett namn för hello Event Hub.
    
    b. Den här självstudien anges **antalet partitioner** och **meddelandet kvarhållning** toohello standardvärden.
    
    c. Ange **avbilda** för**på**. Ange hello **tidsfönstret** (hur ofta toocapture) och **storlek fönstret** (data storlek toocapture). 
    
    d. För **avbilda providern**väljer **Azure Data Lake Store** och hello väljer hello Data Lake Store som du skapade tidigare. För **Data Lake sökvägen**, ange hello namnet på hello-mappen som du skapade i hello Data Lake Store-konto. Du behöver bara tooprovide hello relativ sökväg toohello mapp.

    e. Lämna hello **exempel avbilda filformat namn** toohello standardvärdet. Det här alternativet styr hello mappstruktur som skapas under hello avbilda mapp.

    f. Klicka på **Skapa**.

## <a name="test-hello-setup"></a>Inställningar för hello

Nu kan du testa hello lösning genom att skicka data toohello Azure Event Hub. Följ instruktionerna för hello på [skicka händelser tooAzure Händelsehubbar](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). När du börjar skicka hello data Se du hello om data som visas i Data Lake Store använder hello mappstruktur som du angav. Till exempel finns en mappstruktur som visas i följande skärmbild i Data Lake Store hello.

![Exempel på EventHub-data i Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "exempel EventHub-data i Data Lake Store")

> [!NOTE]
> Även om du inte har meddelanden som skickas till Händelsehubbar skriver Händelsehubbar tom filer med bara hello huvuden i hello Data Lake Store-konto. hello skrivs på hello samma tidsintervall som du angav när du skapar hello Händelsehubbar.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Analysera data i Data Lake Store

När hello data i Data Lake Store kan köra du analytiska jobb tooprocess och matar hello-data. Se [USQL Avro exempel](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) om hur toodo detta med hjälp av Azure Data Lake Analytics.
  

## <a name="see-also"></a>Se även
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Kopiera data från Azure Storage BLOB tooData Datasjölager](data-lake-store-copy-data-azure-storage-blob.md)
