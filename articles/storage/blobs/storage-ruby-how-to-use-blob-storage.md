---
title: "aaaHow toouse Blob storage (objektlagring) från Ruby | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 776e7d788e69d4960f8dde0b783513f6b39b7a47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ruby"></a>Hur toouse Blob storage från Ruby
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.

Den här guiden visar hur tooperform vanliga scenarier med Blob storage. hello exempel skrivs med hello Ruby API. hello beskrivs scenarier där **överför, lista, hämtar** och **bort** blobbar.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Skapa ett Ruby-program
Skapa ett Ruby program. Instruktioner finns i [Ruby på spår webbprogram på en Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurera ditt program tooaccess lagring
toouse Azure Storage, behöver du toodownload och Använd hello Ruby azure-paketet som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.

### <a name="use-rubygems-tooobtain-hello-package"></a>Använd RubyGems tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).
2. Skriv ”symbolen installera azure” i hello kommandot fönstret tooinstall hello symbolen och beroenden.

### <a name="import-hello-package"></a>Importera hello-paket
Med hjälp av valfri textredigerare, Lägg till hello följande toohello överkant hello Ruby filen där du ska toouse lagring:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure Storage-anslutning
hello azure modulen läser hello miljövariabler **AZURE\_lagring\_konto** och **AZURE\_lagring\_ACCESS_KEY** för information krävs tooconnect tooyour Azure storage-konto. Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation innan du använder **Azure::Blob::BlobService** med hello följande kod:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

tooobtain dessa värden från en klassiska eller Resource Manager lagring konto i hello Azure-portalen:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Navigera toohello storage-konto som du vill toouse.
3. I hello inställningsbladet på hello höger klickar du på **åtkomstnycklar**.
4. I hello åtkomst bladet nycklar som visas, visas hello åtkomstnyckel 1 och åtkomstnyckel 2. Du kan använda någon av dessa.
5. Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp.

## <a name="create-a-container"></a>Skapa en behållare
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Hej **Azure::Blob::BlobService** objekt kan du arbeta med behållare och blobbar. toocreate en behållare använder hello **skapa\_container()** metod.

hello följande exempel skapar en behållare eller skriver ut hello fel om sådana finns.

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Om du vill toomake hello filer i hello behållaren offentliga kan ange du hello behållaren behörigheter.

Du kan bara ändra hello <strong>skapa\_container()</strong> anropet toopass hello **: offentliga\_åtkomst\_nivå** alternativ:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

Giltiga värden för hello **: offentliga\_åtkomst\_nivå** alternativ är:

* **BLOB:** anger offentlig läsbehörighet för blobbar. BLOB-data i den här behållaren kan läsas via anonym begäran, men behållardata är inte tillgänglig. Klienter kan inte räkna upp blobbar i behållaren hello via anonym begäran.
* **behållare:** anger fullständig offentlig läsbehörighet för behållaren och blob-data. Klienter kan räkna upp blobbar i behållaren hello via anonym begäran, men det går inte att räkna upp behållare i hello storage-konto.

Alternativt, du kan ändra hello offentliga åtkomstnivån för en behållare med hjälp av **ange\_behållare\_acl()** åtkomstnivå för metoden toospecify hello offentliga.

Hej följande kodexempel ändringar hello offentliga åtkomstnivå för**behållare**:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
tooupload innehåll tooa blob, Använd hello **skapa\_block\_blob()** metoden toocreate hello blob, använda en fil eller en sträng som hello innehållet i hello blob.

hello följande kod överför hello fil **test.png** som en ny blob med namnet ”-avbildningsbloben” i hello behållaren.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello behållare använder **list_containers()** metod.
toolist hello blobbar i en behållare använder **lista\_blobs()** metod.

Detta matar ut hello URL: er för alla hello BLOB i alla hello-behållare för hello-kontot.

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a>Ladda ned blobbar
toodownload blobbar använda hello **hämta\_blob()** metoden tooretrieve hello innehållet.

hello följande kodexempel visas hur du använder **hämta\_blob()** toodownload hello innehållet i ”avbildningsbloben” och skriver den tooa lokal fil.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Ta bort en Blobb
Slutligen toodelete blob använda hello **ta bort\_blob()** metod. hello följande kodexempel visar hur toodelete en blob.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Nästa steg
toolearn om mer komplexa lagringsuppgifter följa dessa länkar:

* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage/)
* [Azure SDK för Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub
* [Överföra data med kommandoradsverktyget Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

