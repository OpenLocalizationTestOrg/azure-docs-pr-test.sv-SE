---
title: aaaUsing hello Azure CLI 2.0 med Azure Storage | Microsoft Docs
description: "Lär dig hur toouse hello Azure-kommandoradsgränssnittet (Azure CLI) 2.0 med Azure Storage toocreate och hantera storage-konton och arbeta med Azure-blobbar och filer. hello Azure CLI 2.0 är ett verktyg för flera plattformar som skrivits i Python."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: marsma
ms.openlocfilehash: 14e6eb0c913676380c90a72563276245e7f08aa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-20-with-azure-storage"></a>Med Azure Storage hello Azure CLI 2.0

hello öppen källkod, plattformsoberoende Azure CLI 2.0 innehåller en uppsättning kommandon för att arbeta med hello Azure-plattformen. Det ger mycket hello samma funktioner som finns i hello [Azure-portalen](https://portal.azure.com), inklusive omfattande dataåtkomst.

I den här guiden visar vi dig hur toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform flera uppgifter arbeta med resurserna i Azure Storage-konto. Vi rekommenderar att du hämtar och installerar eller uppgraderar toohello senaste versionen av hello CLI 2.0 innan du använder den här guiden.

hello exemplen i hello handboken förutsätter hello användning av hello Bash shell på Ubuntu, men andra plattformar ska utföra på samma sätt. 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a>Krav
Den här handboken förutsätts att du förstår hello grundläggande begrepp för Azure Storage. Det förutsätts även att du är kan toosatisfy hello skapa krav som anges nedan för Azure och hello Storage-tjänst.

### <a name="accounts"></a>Konton
* **Azure-konto**: Om du inte redan har en Azure-prenumeration [skapa ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).
* **Lagringskonto**: Se [skapar ett lagringskonto](storage-create-storage-account.md#create-a-storage-account) i [Om Azure-lagringskonton](storage-create-storage-account.md).

### <a name="install-hello-azure-cli-20"></a>Installera hello Azure CLI 2.0

Hämta och installera hello Azure CLI 2.0 genom att följa hello instruktionerna [installera Azure CLI 2.0](/cli/azure/install-az-cli2).

> [!TIP]
> Om du har problem med installation hello kolla hello [felsökning för installationen](/cli/azure/install-az-cli2#installation-troubleshooting) avsnittet hello artikel och hello [installera felsökning](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) guide på GitHub.
>

## <a name="working-with-hello-cli"></a>Arbeta med hello CLI

När du har installerat hello CLI, kan du använda hello `az` i din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) tooaccess hello Azure CLI-kommandona. Typen hello `az` kommandot toosee en fullständig lista över grundläggande hello-kommandon (hello följande exempel på utdata har trunkerats):

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome toohello cool new Azure CLI!

Here are hello base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

Köra hello-kommandot i din kommandoradsgränssnittet `az storage --help` toolist hello `storage` kommandot undergrupper. hello beskrivningar av hello undergrupper ger en översikt över hello funktioner hello Azure CLI tillhandahåller för att arbeta med dina lagringsresurser.

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use hello standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues tooeffectively scale applications according tootraffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-hello-cli-tooyour-azure-subscription"></a>Ansluta hello CLI tooyour Azure-prenumeration

toowork med hello resurser i din Azure-prenumeration måste du först logga i tooyour Azure-konto med `az login`. Det finns flera sätt som du kan logga in:

* **Interaktiv inloggning**:`az login`
* **Logga in med användarnamn och lösenord**:`az login -u johndoe@contoso.com -p VerySecret`
  * Detta fungerar inte med Microsoft-konton eller konton som använder multi-Factor authentication.
* **Logga in med ett huvudnamn för tjänsten**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`

## <a name="azure-cli-20-sample-script"></a>Azure CLI 2.0 exempelskript

Nu ska arbetar vi med ett litet kommandoskript som utfärdar några grundläggande Azure CLI 2.0 kommandon toointeract med Azure Storage-resurser. hello skriptet först skapar en ny behållare på ditt lagringskonto och överför en befintlig fil (som en blob) toothat behållare. Den visar en lista över alla blobbar i behållaren hello och slutligen hämtar hello filen tooa mål på den lokala datorn som du anger.

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating hello container..."
az storage container create --name $container_name

echo "Uploading hello file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing hello blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading hello file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

**Konfigurera och köra hello-skript**

1. Öppna valfri textredigerare, och sedan kopiera och klistra in hello föregående skript i hello-redigeraren.

2. Därefter uppdatera hello skript variabler tooreflect konfigurationsinställningarna. Ersätt hello värden enligt följande:

   * **\<storage_account_name\>**  hello namnet på ditt lagringskonto.
   * **\<storage_account_key\>**  hello primära eller sekundära åtkomstnyckeln för ditt lagringskonto.
   * **\<container_name\>**  ett namn hello nya behållare toocreate, till exempel ”azure-cli-exemplet-container”.
   * **\<blob_name\>**  ett namn för hello i hello behållaren.
   * **\<file_to_upload\>**  hello sökväg toosmall fil på den lokala datorn som ”~ / images/HelloWorld.png”.
   * **\<destination_file\>**  hello mål filsökväg som ”~ / downloadedImage.png”.

3. När du har uppdaterat hello nödvändiga variabler, spara hello skript och avsluta redigeringsprogram. hello nästa steg förutsätter att du har namngett skriptet **my_storage_sample.sh**.

4. Markera hello skriptet som körbara, om det behövs:`chmod +x my_storage_sample.sh`

5. Kör hello-skriptet. Till exempel i Bash:`./my_storage_sample.sh`

Du bör se utdata liknande toohello nedan och hello  **\<destination_file\>**  du angav i hello skript ska visas på den lokala datorn.

```
Creating hello container...
{
  "created": true
}
Uploading hello file...
Percent complete: %100.0
Listing hello blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading hello file...
Name
---------
README.md
Done
```

> [!TIP]
> hello föregående utdata är i **tabell** format. Du kan ange vilka utdata format toouse genom att ange hello `--output` argument i CLI-kommandon eller Ställ in den globalt med `az configure`.
>

## <a name="manage-storage-accounts"></a>Hantera lagringskonton

### <a name="create-a-new-storage-account"></a>Skapa ett nytt lagringskonto
toouse Azure Storage, du behöver ett lagringskonto. Du kan skapa ett nytt Azure Storage-konto när du har konfigurerat datorn för[ansluter tooyour prenumeration](#connect-to-your-azure-subscription).

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* `--location`[Krävs]: plats. Till exempel ”USA, västra”.
* `--name`[Krävs]: hello lagringskontonamn. hello-namnet måste bestå av 3 too24 tecken och Använd endast gemener alfanumeriska tecken.
* `--resource-group`[Krävs]: namnet på resursgruppen.
* `--sku`[Krävs]: hello SKU-lagringskontot. Tillåtna värden:
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a>Ange standardkontot för Azure storage miljövariabler
Du kan ha flera lagringskonton i Azure-prenumerationen. tooselect en av dem toouse för alla efterföljande lagring kommandon, kan du ange de här miljövariablerna:

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Det är ett annat sätt tooset standardkontot för lagring med hjälp av en anslutningssträng. Först hämta hello anslutningssträngen med hello `show-connection-string` kommando:

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

Sedan kopiera hello utdata anslutningssträngen och ange hello `AZURE_STORAGE_CONNECTION_STRING` miljövariabeln (du kanske måste tooenclose hello anslutningssträngen med citattecken):

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> Alla exempel i hello följande avsnitt i den här artikeln förutsätter att du har ställt in hello `AZURE_STORAGE_ACCOUNT` och `AZURE_STORAGE_ACCESS_KEY` miljövariabler.
>

## <a name="create-and-manage-blobs"></a>Skapa och hantera blobbar
Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS. Det här avsnittet förutsätter att du redan är bekant med Azure Blob storage-koncept. Detaljerad information finns i [komma igång med Azure Blob storage med hjälp av .NET](../blobs/storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](/rest/api/storageservices/blob-service-concepts).

### <a name="create-a-container"></a>Skapa en behållare
Varje blobb i Azure-lagring måste vara i en behållare. Du kan skapa en behållare med hello `az storage container create` kommando:

```azurecli
az storage container create --name <container_name>
```

Du kan ange en av tre nivåer för läsbehörighet för en ny behållare genom att ange hello valfria `--public-access` argument:

* `off`(standard): behållardata privata toohello konto äger.
* `blob`: Offentlig läsbehörighet för blobbar.
* `container`: Offentliga Läs- och åtkomst toohello hela behållaren.

Mer information finns i [Hantera anonym läsbehörighet toocontainers och blobbar](../blobs/storage-manage-access-to-resources.md).

### <a name="upload-a-blob-tooa-container"></a>Överför en tooa blob-behållare
Azure Blob storage stöder block, Lägg till och sidblobbar. Överför blobbar tooa behållaren med hjälp av hello `blob upload` kommando:

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 Som standard hello `blob upload` kommandot överför *.vhd filer toopage blobbar eller blockblobbar på annat sätt. toospecify en annan skriver när du laddar upp en blobb kan du använda hello `--type` argumentet--tillåtna värden är `append`, `block`, och `page`.

 Mer information om hello annan blob-typer finns [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).


### <a name="download-a-blob-from-a-container"></a>Hämta en blob från en behållare
Det här exemplet visar hur toodownload en blob från en behållare:

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare

Visa hello blobbar i en behållare med hello [az storage blob listan](/cli/azure/storage/blob#list) kommando.

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a>Kopiera BLOB
Du kan kopiera blobar i eller mellan lagringskonton och regioner asynkront.

hello visar exemplet nedan hur toocopy blobbar från en lagring kontot tooanother. Vi först skapa en behållare i hello källa storage-konto, ange offentlig läsbehörighet för dess blobbar. Därefter överföra vi en fil toohello behållare och slutligen kopiera hello blob från behållaren till en behållare i hello destinationslagringskontot.

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob toocontainer in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account toodestination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

I ovanstående exempel hello finnas hello målbehållare redan i hello destinationslagringskontot för hello Kopiera åtgärden toosucceed. Dessutom hello källa blob anges i hello `--source-uri` argument måste inkludera en token för delad åtkomst signatur (SAS), eller vara allmänt tillgänglig i det här exemplet.

### <a name="delete-a-blob"></a>Ta bort en blob
toodelete blob använda hello `blob delete` kommando:

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a>Skapa och hantera filresurser
Azure File storage erbjuder delad lagring för program som använder hello Server Message Block (SMB) protokollet. Microsoft Azure-datorer och molntjänster, samt lokala program kan dela fildata via monterade resurser. Du kan hantera filresurser och fildata via hello Azure CLI. Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](../storage-dotnet-how-to-use-files.md) eller [hur toouse Azure File storage med Linux](../storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Skapa en filresurs
En filresurs i Azure är en SMB-filresurs i Azure. Alla kataloger och filer måste skapas i en filresurs. Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer, upp toohello kapacitetsbegränsningar för hello storage-konto. hello följande exempel skapar en filresurs med namnet **minresurs**.

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a>Skapa en katalog
En katalog innehåller en hierarkisk struktur i ett Azure-filresursen. hello följande exempel skapas en katalog med namnet **MinMapp** i hello filresurs.

```azurecli
az storage directory create --name myDir --share-name myshare
```

En katalogsökväg kan innehålla flera nivåer, till exempel **dir1/dir2**. Dock måste du kontrollera att alla överordnade kataloger finns innan du skapar en underkatalog. Till exempel för sökvägen **dir1/dir2**, måste du först skapa directory **dir1**, skapa katalog **dir2**.

### <a name="upload-a-local-file-tooa-share"></a>Ladda upp en lokal fil tooa resurs
hello följande exempel överför en fil från **~/temp/samplefile.txt** tooroot av hello **minresurs** filresurs. Hej `--source` argumentet anger hello befintliga lokala filen tooupload.

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

Du kan ange en sökväg i hello resursen tooupload hello filen tooan befintliga directory hello resursen som directory skapas:

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

En fil i hello resursen kan vara upp too1 TB i storlek.

### <a name="list-hello-files-in-a-share"></a>Lista över hello filer i en resurs
Du kan lista filer och kataloger på en resurs med hjälp av hello `az storage file list` kommando:

```azurecli
# List hello files in hello root of a share
az storage file list --share-name myshare --output table

# List hello files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List hello files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a>Kopiera filer      
Du kan kopiera en fil för tooanother, en tooa blob-fil eller en tooa blob-fil. Till exempel toocopy en katalog i tooa i en annan resurs:        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a>Nästa steg
Här följer några ytterligare resurser för att lära dig mer om hur du arbetar med hello Azure CLI 2.0.

* [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure CLI 2.0-kommandoreferens](/cli/azure)
* [Azure CLI 2.0 på GitHub](https://github.com/Azure/azure-cli)
