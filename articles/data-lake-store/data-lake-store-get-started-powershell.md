---
title: "aaaUse PowerShell tooget igång med Azure Data Lake Store | Microsoft Docs"
description: "Använda Azure PowerShell toocreate ett Data Lake Store-konto och utföra grundläggande åtgärder"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 9c958bfd63e412ec0b0a4113a149f61aee026bc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Kom igång med Azure Data Lake Store med hjälp av Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Lär dig hur toouse Azure PowerShell toocreate ett Azure Data Lake lagrar konto och utföra grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, ta bort ditt konto, osv. Mer information om Data Lake Store finns i [Översikt över Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Installera Azure PowerShell 1.0 eller senare**. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="authentication"></a>Autentisering
Den här artikeln använder en enklare metod för autentisering med Data Lake Store var du befinner dig ange tooenter dina Azure-autentiseringsuppgifter. hello åtkomst nivå tooData Lake Store-konto och filsystem regleras sedan hello åtkomstnivån för hello inloggad användare. Men det finns andra metoder som korrekt tooauthenticate med Data Lake Store, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**. Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Skapa ett Azure Data Lake Store-konto
1. På skrivbordet och öppna ett nytt Windows PowerShell-fönster och ange hello följande fragment toolog i tooyour Azure-konto, ange hello prenumerationen och registrera hello Data Lake Store-providern. När du tillfrågas toolog i, se till att logga in som en hello prenumerationens admininistratörer/ägare:

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Ett Azure Data Lake Store-konto är kopplat till en resursgrupp i Azure. Börja med att skapa en Azure-resursgrupp.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Skapa en Azure-resursgrupp](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Skapa en Azure-resursgrupp")
3. Skapa ett Azure Data Lake Store-konto. hello-namn som du anger får bara innehålla gemena bokstäver och siffror.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Skapa ett Azure Data Lake Store-konto](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Skapa ett Azure Data Lake Store-konto")
4. Kontrollera att hello kontot har skapats.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    hello utdata för detta bör vara **SANT**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Skapa katalogstrukturer i din Azure Data Lake Store
Du kan skapa kataloger under toomanage din Azure Data Lake Store-konto och lagra data.

1. Ange en rotkatalog.

        $myrootdir = "/"
2. Skapa en ny katalog som kallas **mynewdirectory** under hello angivna roten.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Kontrollera att den nya katalogen hello har skapats.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Du bör se utdata som liknar hello följande:

    ![Verifiera katalog](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verifiera katalog")

## <a name="upload-data-tooyour-azure-data-lake-store"></a>Ladda upp data tooyour Azure Data Lake Store
Du kan ladda upp din tooData Datasjölager direkt vid hello nivå eller tooa rotkatalog som du skapade i hello-konto. hello fragmenten nedan visar hur tooupload vissa toohello datakatalog exempel (**mynewdirectory**) du skapade i föregående avsnitt i hello.

Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Hämta hello-filen och lagra den på en lokal katalog på din dator, till exempel C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Byt namn, hämta och ta bort data från din Data Lake Store
toorename en fil, Använd hello följande kommando:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

toodownload en fil, Använd hello följande kommando:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

toodelete en fil, Använd hello följande kommando:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

När du uppmanas, anger **Y** toodelete hello-objektet. Om du har mer än en fil toodelete kan du ge alla hello sökvägar avgränsade med kommatecken.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Ta bort ditt Azure Data Lake Store-konto
Använd följande kommando toodelete hello ditt Data Lake Store-konto.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

När du uppmanas, anger **Y** toodelete hello-konto.

## <a name="performance-guidance-while-using-powershell"></a>Prestandavägledning när du använder PowerShell

Nedan visas hello viktigaste inställningar som kan vara ögonen öppna tooget hello bästa prestanda när du använder PowerShell toowork med Data Lake Store:

| Egenskap            | Standard | Beskrivning |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Den här parametern kan du toochoose hello antalet parallella trådar för att överföra eller hämta varje fil. Det här representerar hello högsta antal trådar som kan tilldelas per fil, men du får mindre trådar beroende på scenario (t.ex. Om du överför en fil på 1 KB får du en tråd även om du fråga efter 20 trådar).  |
| ConcurrentFileCount | 10      | Den här parametern är specifikt för att ladda upp och ned mappar. Den här parametern anger hello antalet samtidiga filer som kan överföras eller hämtas. Det här representerar hello maximalt antal samtidiga filer som kan överföras eller hämtas åt gången, men du får mindre samtidighet beroende på scenario (t.ex. Om du överför två filer, får du två samtidiga filöverföringar även om du uppmanas 15). |

**Exempel**

Det här kommandot laddar ned filer från Azure Data Lake Store toohello användarens lokala enhet med 20 trådar per fil- och 100 samtidiga filer.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-hello-value-tooset-for-these-parameters"></a>Hur vet hello värdet tooset för dessa parametrar?

Här är några riktlinjer som du kan använda.

* **Steg 1: Bestäm hello Totalt antal trådar** -bör du börja med beräkning av hello totala tråd antal toouse. Som en generell riktlinje bör du använda 6 trådar för varje fysisk kärna.

        Total thread count = total physical cores * 6

    **Exempel**

    Under förutsättning att du kör hello PowerShell-kommandon från en D14 virtuell dator som har 16 kärnor

        Total thread count = 16 cores * 6 = 96 threads


* **Steg 2: Beräkna PerFileThreadCount** -vi beräkna vår PerFileThreadCount baserat på hello storlek på hello-filer. För filer som är mindre än 2,5 GB finns det inget behov av toochange den här parametern eftersom hello standardvärdet 10 är tillräckligt. För filer som är större än 2,5 GB bör du använda 10 trådar som hello bas för hello första 2,5 GB och Lägg till 1 tråd för varje ytterligare 256 MB ökning av filstorlek. Om du kopierar en mapp med många olika filstorlekar bör du överväga att gruppera dem enligt liknande filstorlekar. Olika stora filstorlekar kan orsaka bristfälliga prestanda. Om detta inte är möjligt toogroup liknande filstorlekar bör du ange PerFileThreadCount baserat på hello största filstorlek.

        PerFileThreadCount = 10 threads for hello first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Exempel**

    Anta att du har 100 filer från 1GB too10GB och använder vi hello 10GB som hello största filstorlek för formeln som skulle läsa som hello nedan.

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **Steg 3: Beräkna ConcurrentFilecount** -Använd hello Totalt antal trådar och PerFileThreadCount toocalculate ConcurrentFileCount baserat på hello följande formel.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Exempel**

    Baserat på hello exempelvärden har vi använt

        96 = 40 * ConcurrentFileCount

    Därför **ConcurrentFileCount** är **2.4**, där vi kan avrunda för**2**.

### <a name="further-tuning"></a>Ytterligare justering

Du kan kräva att ytterligare finjustera eftersom det finns en mängd filen storlekar toowork med. hello ovan beräkning fungerar bra om alla eller de flesta hello filer är större och närmare toohello 10GB intervall. Om det finns i stället många olika filstorlekar med många filer som är mindre kan du minska PerFileThreadCount. Vi kan öka ConcurrentFileCount genom att minska hello PerFileThreadCount. Så om vi antar att de flesta av våra filer har mindre hello 5GB intervallet kan vi gör nytt våra beräkning:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

Därför **ConcurrentFileCount** kommer nu att 96/20, vilket är 4.8 avrundas för**4**.

Du kan fortsätta tootune inställningarna genom att ändra hello **PerFileThreadCount** uppåt och nedåt beroende på hello distribution av din filstorlekar.

### <a name="limitation"></a>Begränsning

* **Antalet filer som är mindre än ConcurrentFileCount**: om hello antalet filer som du överför är mindre än hello **ConcurrentFileCount** som du beräknade sedan minska  **ConcurrentFileCount** toobe toohello lika många filer. Du kan använda alla återstående trådar tooincrease **PerFileThreadCount**.

* **För många trådar**: Om du ökar tråd räkna för mycket utan att öka klusterstorleken på din, hello risk för försämrade prestanda. Det kan finnas konkurrens problem när du växlar kontext på hello CPU.

* **Otillräcklig samtidighet**: om hello samtidighet räcker inte och klustret kanske för liten. Du kan öka hello antalet noder i klustret som ger mer samtidighet.

* **Begränsningsfel**: Om konkurrensen är för hög kan det leda till begränsningsfel. Om du ser begränsning fel, ska du minskar hello eller kontakta oss.

## <a name="next-steps"></a>Nästa steg
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

