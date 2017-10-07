---
title: "aaaStream data från Stream Analytics i Data Lake Store | Microsoft Docs"
description: "Använda Azure Stream Analytics toostream data i Azure Data Lake Store"
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 68c727d4807db0abe6efa90145d68c78902eb789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Strömma data från Azure Storage Blob till Data Lake Store med Azure Stream Analytics
I den här artikeln du lära dig hur toouse Azure Data Lake lagrar som utdata för ett Azure Stream Analytics-jobb. Den här artikeln visar ett enkelt scenario som läser data från en Azure Storage blob (indata) och skrivningar hello-tooData Datasjölager (utdata).

> [!NOTE]
> För tillfället skapande och konfiguration av Data Lake Store matar ut för Stream Analytics stöds endast i hello [klassiska Azure-portalen](https://manage.windowsazure.com). Därför kan använder vissa delar av den här kursen hello klassiska Azure-portalen.
>
>

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Storage-konto**. Du använder en blob-behållare från detta konto tooinput data för ett Stream Analytics-jobb. Den här självstudiekursen förutsätter att du har ett lagringskonto som kallas **storageforasa** och en behållare i hello kontot kallas **storageforasacontainer**. När du har skapat hello behållare, ladda upp en exempel data filen tooit. 
  
* **Azure Data Lake Store-konto**. Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md). Antar vi att du har ett Data Lake Store-konto som kallas **asadatalakestore**. 

## <a name="create-a-stream-analytics-job"></a>Skapa ett Stream Analytics-jobb
Börja med att skapa ett Stream Analytics-jobb som innehåller ingen källa och ett mål för utdata. I den här självstudien hello källan är en Azure blob-behållaren och hello målet är Data Lake Store.

1. Logga in toohello [Azure Portal](https://portal.azure.com).

2. Hello till vänster och klicka på **Stream Analytics-jobb**, och klicka sedan på **Lägg till**.

    ![Skapa ett Stream Analytics-jobbet](./media/data-lake-store-stream-analytics/create.job.png "skapa ett Stream Analytics-jobb")

    > [!NOTE]
    > Kontrollera att du skapar jobbet i hello samma region som hello storage-konto eller du påförs extra kostnader för att flytta data mellan regioner.
    >

## <a name="create-a-blob-input-for-hello-job"></a>Skapa en Blob-inmatning för hello jobbet

1. Öppna hello för hello Stream Analytics-jobbet i hello vänstra rutan klickar du på hello **indata** fliken och klicka sedan på **Lägg till**.

    ![Lägg till ett jobb för inkommande tooyour](./media/data-lake-store-stream-analytics/create.input.1.png "lägga till ett inkommande tooyour-jobb")

2. På hello **nya indata** bladet ange hello följande värden.

    ![Lägg till ett jobb för inkommande tooyour](./media/data-lake-store-stream-analytics/create.input.2.png "lägga till ett inkommande tooyour-jobb")

    * För **indata alias**, ange ett unikt namn för hello jobbet indata.
    * För **typ av datakälla**väljer **dataströmmen**.
    * För **källa**väljer **Blob storage**.
    * För **prenumeration**väljer **använda blob storage från aktuell prenumeration**.
    * För **lagringskonto**, Välj hello storage-konto som du har skapat som en del av hello krav. 
    * För **behållare**väljer hello-behållare som du skapade i hello valt storage-konto.
    * För **händelse serialiseringsformat**väljer **CSV**.
    * För **avgränsare**väljer **fliken**.
    * För **kodning**väljer **UTF-8**.

    Klicka på **Skapa**. hello portal nu lägger till hello indata och testar hello anslutning tooit.


## <a name="create-a-data-lake-store-output-for-hello-job"></a>Skapa ett Data Lake Store-utdata för hello jobb

1. Öppna hello hello Stream Analytics-jobbet på sidan, klicka på hello **utdata** fliken och klicka sedan på **Lägg till**.

    ![Lägg till ett jobb för utdata tooyour](./media/data-lake-store-stream-analytics/create.output.1.png "lägga till en utgående tooyour jobb")

2. På hello **nya utdata** bladet ange hello följande värden.

    ![Lägg till ett jobb för utdata tooyour](./media/data-lake-store-stream-analytics/create.output.2.png "lägga till en utgående tooyour jobb")

    * För **kolumnalias**, ange ett unikt namn för hello jobbutdata. Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Data Lake Store.
    * För **Sink**väljer **Datasjölager**.
    * Du kan ange tooauthorize komma åt tooData Lake Store-konto. Klicka på **auktorisera**.

3. På hello **nya utdata** bladet fortsätta tooprovide hello följande värden.

    ![Lägg till ett jobb för utdata tooyour](./media/data-lake-store-stream-analytics/create.output.3.png "lägga till en utgående tooyour jobb")

    * För **kontonamn**, Välj hello Data Lake Store-konto som du redan skapat där du vill att hello jobbet utdata toobe skickas till.
    * För **prefix sökvägar**, ange en fil sökvägen toowrite filerna i hello angett Data Lake Store-konto.
    * För **datumformat**, om du har använt en datumtoken i hello prefix sökväg, kan du välja hello datumformat där filerna ordnas.
    * För **tidsformat**, om du har använt en tid token i hello prefix sökväg, ange hello tidsformat där filerna ordnas.
    * För **händelse serialiseringsformat**väljer **CSV**.
    * För **avgränsare**väljer **fliken**.
    * För **kodning**väljer **UTF-8**.
    
    Klicka på **Skapa**. hello portal nu lägger till hello utdata och testar hello anslutning tooit.
    
## <a name="run-hello-stream-analytics-job"></a>Kör hello Stream Analytics-jobbet

1. toorun Stream Analytics-jobbet, måste du köra en fråga från hello **frågan** fliken. Den här självstudiekursen kommer du kan köra hello exempelfråga genom att ersätta hello-platshållare med hello jobbet alias för inkommande och utgående, enligt hello skärmdumpen nedan.

    ![Kör frågan](./media/data-lake-store-stream-analytics/run.query.png "kör frågan")

2. Klicka på **spara** från hello överkant hello-skärmen och sedan från hello **översikt** klickar du på **starta**. Hello dialogrutan Välj **anpassad tid**, och sedan ange hello aktuellt datum och tid.

    ![Ange jobbtiden för](./media/data-lake-store-stream-analytics/run.query.2.png "ange jobbtiden för")

    Klicka på **starta** toostart hello jobb. Det kan ta upp tooa några minuter toostart hello jobb.

3. tootrigger hello toopick hello jobbdata från hello blob kopiera en exempel data filen toohello blob-behållare. Du kan hämta en exempeldatafil från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). För den här kursen ska vi kopiera hello filen **vehicle1_09142014.csv**. Du kan använda olika klienter som [Azure Lagringsutforskaren](http://storageexplorer.com/), tooupload data tooa blob-behållare.

4. Från hello **översikt** fliken, under **övervakning**, se hur hello data bearbetades.

    ![Övervakningsjobb](./media/data-lake-store-stream-analytics/run.query.3.png "Övervakningsjobb")

5. Slutligen kan du kontrollera att hello utdata för jobbet är tillgängliga i hello Data Lake Store-konto. 

    ![Kontrollera utdata](./media/data-lake-store-stream-analytics/run.query.4.png "Kontrollera utdata")

    Observera att hello utdata skriftliga tooa sökvägen som anges i hello Data Lake Store utdata inställningar i hello Data Explorer-fönstret (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Se även
* [Skapa ett HDInsight-kluster toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
