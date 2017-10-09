---
title: aaaUsing hello Azure CLI 1.0 med Azure Storage | Microsoft Docs
description: "Lär dig hur toouse hello Azure-kommandoradsgränssnittet (Azure CLI) 1.0 med Azure Storage toocreate och hantera storage-konton och arbeta med Azure-blobbar och filer. hello Azure CLI är ett verktyg för flera plattformar"
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: 786f2be64875f5094f09fd6e4a47532889c3a82f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-10-with-azure-storage"></a>Med Azure Storage hello Azure CLI 1.0

## <a name="overview"></a>Översikt

hello Azure CLI innehåller en uppsättning med öppen källkod, plattformsoberoende kommandon för att arbeta med hello Azure-plattformen. Det ger mycket hello samma funktioner som finns i hello [Azure-portalen](https://portal.azure.com) samt som omfattande funktioner för dataåtkomst.

I den här handboken vi ska visa hur toouse [Azure-kommandoradsgränssnittet (Azure CLI)](../cli-install-nodejs.md) tooperform en rad olika uppgifter för utveckling och administration med Azure Storage. Vi rekommenderar att du hämtar och installerar eller uppgraderar toohello senaste Azure CLI innan du använder den här guiden.

Den här handboken förutsätts att du förstår hello grundläggande begrepp för Azure Storage. hello guiden ger ett antal skript toodemonstrate hello användning av hello Azure CLI med Azure Storage. Vara säker på att tooupdate hello skriptvariabler baserat på din konfiguration innan du kör varje skript.

> [!NOTE]
> hello guiden visar hello Azure CLI kommando- och exempel för klassiska lagringskonton. Se [Using hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) för Azure CLI-kommandona för Resource Manager storage-konton.
>
>

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-hello-azure-cli-in-5-minutes"></a>Kom igång med Azure Storage och hello Azure CLI i 5 minuter
Den här guiden använder Ubuntu exempel, men andra operativsystemplattformar ska utföra på samma sätt.

**Nya tooAzure:** hämta Microsoft Azure-prenumeration och ett Microsoft-konto som är kopplade till den prenumerationen. Mer information om köpalternativ för Azure finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/), [köpa alternativ](https://azure.microsoft.com/pricing/purchase-options/), och [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/) (för medlemmar i MSDN, Microsoft Partner Network, BizSpark och att andra Microsoft-program).

Se [Tilldela administratörsroller i Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) för mer information om Azure-prenumerationer.

**När du har skapat ett Microsoft Azure-prenumeration och konto:**

1. Hämta och installera hello Azure CLI hello instruktionerna som beskrivs i [installera hello Azure CLI](../cli-install-nodejs.md).
2. När hello Azure CLI har installerats, kommer du att kunna toouse hello azure kommando från din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) tooaccess hello Azure CLI-kommandona. Typen hello _azure_ kommandot och du bör se hello följande utdata.

    ![Azure kommandoutdata][Image1]
3. Skriv i hello kommandoradsgränssnittet `azure storage` toolist ut alla hello azure storage-kommandon och få en första intryck av hello funktioner hello Azure CLI tillhandahåller. Du kan skriva kommandonamn med **-h** parameter (till exempel `azure storage share create -h`) toosee information om kommandosyntax.
4. Nu får du ett enkelt skript som visar grundläggande Azure CLI-kommandon tooaccess Azure Storage. hello skriptet först ber dig tooset två variabler för ditt lagringskonto och en nyckel. Hello skript ska skapa en ny behållare i den här nya storage-konto och sedan ladda upp en befintlig avbildning fil (blob) toothat behållare. När hello-skriptet visar alla blobbar i behållaren, hämtas hello image-filen toohello målkatalogen som finns på hello lokala datorn.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating hello container..."
    azure storage container create $container_name

    echo "Uploading hello image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing hello blobs..."
    azure storage blob list $container_name

    echo "Downloading hello image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. Öppna din önskade textredigerare (vim till exempel) i den lokala datorn. Ange hello ovan skriptet i en textredigerare.
6. Du måste nu tooupdate hello skriptvariabler baserat på inställningarna.

   * **< Storage_account_name >** använda hello får namn i skriptet hello eller ange ett nytt namn för ditt lagringskonto. **Viktigt:** hello namnet på hello storage-konto måste vara unikt i Azure. Det måste vara gemena för!
   * **< storage_account_key >** hello åtkomstnyckeln för ditt lagringskonto.
   * **< Container_name >** använda hello får namn i skriptet hello eller ange ett nytt namn för din behållare.
   * **< Image_to_upload >** ange en sökväg tooa bild på den lokala datorn, till exempel ”: ~ / images/HelloWorld.png”.
   * **< Destination_folder >** ange sökvägen tooa lokal katalog toostore filer som hämtas från Azure Storage: ”~/downloadImages”.
7. När du har uppdaterat hello nödvändiga variabler i vim trycker du på tangentkombinationer `ESC`, `:`, `wq!` toosave hello skript.
8. toorun det här skriptet kan bara typen hello filnamn i hello bash-konsolen. När skriptet körs, bör du ha en lokal målmapp som innehåller hello hämtade image-filen. hello följande skärmbild visar ett exempel på utdata:

När hello skriptet körs, bör du ha en lokal målmapp som innehåller hello hämtade image-filen.

## <a name="manage-storage-accounts-with-hello-azure-cli"></a>Hantera storage-konton med hello Azure CLI
### <a name="connect-tooyour-azure-subscription"></a>Ansluta tooyour Azure-prenumeration
Medan de flesta kommandon för lagring av hello fungerar utan en Azure-prenumeration rekommenderar vi du tooconnect tooyour prenumeration från hello Azure CLI. tooconfigure hello Azure CLI toowork med din prenumeration gör hello i [ansluta tooan Azure-prenumeration från hello Azure CLI](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Skapa ett nytt lagringskonto
toouse Azure storage, behöver ett lagringskonto. Du kan skapa ett nytt Azure storage-konto när du har konfigurerat din dator tooconnect tooyour prenumeration.

```azurecli
azure storage account create <account_name>
```

hello namnet på ditt lagringskonto måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Ange ett standardkonto för Azure storage i miljövariabler
Du kan ha flera lagringskonton i din prenumeration. Du kan välja en av dem och ange den i hello miljövariabler för lagring av alla hello-kommandon i hello samma session. Detta gör att du toorun hello Azure CLI lagring kommandon utan att ange hello storage-konto och nyckeln explicit.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Ett annat sätt tooset standardkontot för lagring med hjälp av anslutningssträngen. Hämta det första hello anslutningssträngen med kommandot:

```azurecli
azure storage account connectionstring show <account_name>
```

Kopiera anslutningssträngen för hello utdata och ange den tooenvironment variabeln:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Skapa och hantera blobbar
Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS. Det här avsnittet förutsätter att du redan är bekant med hello Azure Blob storage-koncept. Detaljerad information finns i [komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Skapa en behållare
Varje blobb i Azure-lagring måste vara i en behållare. Du kan skapa en privat behållare med hello `azure storage container create` kommando:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Det finns tre nivåer av anonym läsbehörighet: **av**, **Blob**, och **behållare**. tooprevent anonym åtkomst till tooblobs parameter för hello behörighet för**av**. Som standard hello ny behållare är privat och kan endast användas av hello Kontoägare. tooallow anonym offentliga läsa åtkomst tooblob resurser, men inte toocontainer metadata eller toohello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**Blob**. tooallow fullständig offentliga läsa komma åt resurser i tooblob, metadata för behållaren och hello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**behållare**. Mer information finns i [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
Azure Blob Storage stöder blockblobbar och sidblobbar. Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).

tooupload blobbar i behållaren tooa, kan du använda hello `azure storage blob upload`. Som standard överför det här kommandot hello lokala filer tooa blockblob. toospecify hello typ för hello blob, du kan använda hello `--blobtype` parameter.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Ladda ned blobbar från en behållare
hello som följande exempel visar hur toodownload BLOB-objekt från en behållare.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Kopiera BLOB
Du kan kopiera blobar i eller mellan lagringskonton och regioner asynkront.

hello visar exemplet nedan hur toocopy blobbar från en lagring kontot tooanother. I det här exemplet skapar vi en behållare där blobbar är offentligt, anonymt tillgänglig.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

Det här exemplet utför en asynkron kopia. Du kan övervaka hello status för varje kopieringen genom att köra hello `azure storage blob copy show` igen.

Observera att hello Käll-URL för hello kopieringsåtgärden måste antingen vara allmänt tillgänglig eller ta en SAS (signatur för delad åtkomst)-token.

### <a name="delete-a-blob"></a>Ta bort en blob
toodelete en blob med hello nedan kommando:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Skapa och hantera filresurser
Azure File storage erbjuder delad lagring för program som använder hello SMB-standardprotokollet. Microsoft Azure-datorer och molntjänster, samt lokala program kan dela fildata via monterade resurser. Du kan hantera filresurser och fildata via hello Azure CLI. Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](storage-dotnet-how-to-use-files.md) eller [hur toouse Azure File storage med Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Skapa en filresurs
En filresurs i Azure är en SMB-filresurs i Azure. Alla kataloger och filer måste skapas i en filresurs. Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer, upp toohello kapacitetsbegränsningar för hello storage-konto. hello följande exempel skapar en filresurs med namnet **minresurs**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Skapa en katalog
En katalog innehåller ett valfritt hierarkisk struktur för ett Azure-filresursen. hello följande exempel skapas en katalog med namnet **MinMapp** i hello filresurs.

```azurecli
azure storage directory create myshare myDir
```

Observera att sökvägen kan innehålla flera nivåer *t.ex.*, **en / b**. Dock måste du kontrollera att alla överordnade kataloger finns. Till exempel för sökvägen **en / b**, måste du skapa katalog **en** först, sedan att skapa katalog **b**.

### <a name="upload-a-local-file-toodirectory"></a>Ladda upp en lokal fil toodirectory
hello följande exempel överför en fil från **~/temp/samplefile.txt** toohello **MinMapp** directory. Redigera hello sökvägen så att den pekar tooa giltig fil på den lokala datorn:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Observera att en fil i hello resursen kan vara upp too1 TB i storlek.

### <a name="list-hello-files-in-hello-share-root-or-directory"></a>Lista över hello filer i hello resursen rot- eller directory
Du kan ange hello filer och underkataloger i en rot-resurs eller en katalog med hello följande kommando:

```azurecli
azure storage file list myshare myDir
```

Observera att hello katalognamnet är valfri för hello lista igen. Om det utelämnas används visar hello kommando hello innehållet hello rotkatalogen hello-resursen.

### <a name="copy-files"></a>Kopiera filer
Från och med version 0.9.8 av Azure CLI kan kopiera du en fil för tooanother, en tooa blob-fil eller en tooa blob-fil. Nedan ser du hur tooperform dessa kopiera åtgärder med hjälp av CLI-kommandona. toocopy en ny katalog i toohello:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

toocopy en blob tooa katalog:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Nästa steg

Du kan hitta Azure CLI 1.0 kommandoreferens för att arbeta med lagringsresurser här:

* [Azure CLI-kommandona i Resource Manager-läge](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Azure CLI-kommandona i Azure Service Management-läge](../cli-install-nodejs.md)

Du kan också använda tootry hello [Azure CLI 2.0](storage-azure-cli.md), våra nästa generations CLI som skrivits i Python, för hello Resource Manager-modellen.

[Image1]: ./media/storage-azure-cli/azure_command.png
