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
# <a name="how-toouse-blob-storage-from-ruby"></a><span data-ttu-id="bb7fd-103">Hur toouse Blob storage från Ruby</span><span class="sxs-lookup"><span data-stu-id="bb7fd-103">How toouse Blob storage from Ruby</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="bb7fd-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bb7fd-104">Overview</span></span>
<span data-ttu-id="bb7fd-105">Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="bb7fd-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="bb7fd-107">BLOB storage är också hänvisade tooas objektlagring.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="bb7fd-108">Den här guiden visar hur tooperform vanliga scenarier med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-108">This guide will show you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="bb7fd-109">hello exempel skrivs med hello Ruby API.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-109">hello samples are written using hello Ruby API.</span></span> <span data-ttu-id="bb7fd-110">hello beskrivs scenarier där **överför, lista, hämtar** och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-110">hello scenarios covered include **uploading, listing, downloading,** and **deleting** blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="bb7fd-111">Skapa ett Ruby-program</span><span class="sxs-lookup"><span data-stu-id="bb7fd-111">Create a Ruby application</span></span>
<span data-ttu-id="bb7fd-112">Skapa ett Ruby program.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-112">Create a Ruby application.</span></span> <span data-ttu-id="bb7fd-113">Instruktioner finns i [Ruby på spår webbprogram på en Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span><span class="sxs-lookup"><span data-stu-id="bb7fd-113">For instructions, see [Ruby on Rails Web application on an Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="bb7fd-114">Konfigurera ditt program tooaccess lagring</span><span class="sxs-lookup"><span data-stu-id="bb7fd-114">Configure your application tooaccess Storage</span></span>
<span data-ttu-id="bb7fd-115">toouse Azure Storage, behöver du toodownload och Använd hello Ruby azure-paketet som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-115">toouse Azure Storage, you need toodownload and use hello Ruby azure package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="bb7fd-116">Använd RubyGems tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="bb7fd-116">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="bb7fd-117">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="bb7fd-117">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="bb7fd-118">Skriv ”symbolen installera azure” i hello kommandot fönstret tooinstall hello symbolen och beroenden.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-118">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="bb7fd-119">Importera hello-paket</span><span class="sxs-lookup"><span data-stu-id="bb7fd-119">Import hello package</span></span>
<span data-ttu-id="bb7fd-120">Med hjälp av valfri textredigerare, Lägg till hello följande toohello överkant hello Ruby filen där du ska toouse lagring:</span><span class="sxs-lookup"><span data-stu-id="bb7fd-120">Using your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="bb7fd-121">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="bb7fd-121">Set up an Azure Storage Connection</span></span>
<span data-ttu-id="bb7fd-122">hello azure modulen läser hello miljövariabler **AZURE\_lagring\_konto** och **AZURE\_lagring\_ACCESS_KEY** för information krävs tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-122">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="bb7fd-123">Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation innan du använder **Azure::Blob::BlobService** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="bb7fd-123">If these environment variables are not set, you must specify hello account information before using **Azure::Blob::BlobService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="bb7fd-124">tooobtain dessa värden från en klassiska eller Resource Manager lagring konto i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="bb7fd-124">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="bb7fd-125">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bb7fd-125">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="bb7fd-126">Navigera toohello storage-konto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-126">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="bb7fd-127">I hello inställningsbladet på hello höger klickar du på **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-127">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="bb7fd-128">I hello åtkomst bladet nycklar som visas, visas hello åtkomstnyckel 1 och åtkomstnyckel 2.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-128">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="bb7fd-129">Du kan använda någon av dessa.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-129">You can use either of these.</span></span>
5. <span data-ttu-id="bb7fd-130">Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-130">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="bb7fd-131">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="bb7fd-131">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="bb7fd-132">Hej **Azure::Blob::BlobService** objekt kan du arbeta med behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-132">hello **Azure::Blob::BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="bb7fd-133">toocreate en behållare använder hello **skapa\_container()** metod.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-133">toocreate a container, use hello **create\_container()** method.</span></span>

<span data-ttu-id="bb7fd-134">hello följande exempel skapar en behållare eller skriver ut hello fel om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-134">hello following code example creates a container or prints hello error if there is any.</span></span>

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

<span data-ttu-id="bb7fd-135">Om du vill toomake hello filer i hello behållaren offentliga kan ange du hello behållaren behörigheter.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-135">If you want toomake hello files in hello container public, you can set hello container's permissions.</span></span>

<span data-ttu-id="bb7fd-136">Du kan bara ändra hello <strong>skapa\_container()</strong> anropet toopass hello **: offentliga\_åtkomst\_nivå** alternativ:</span><span class="sxs-lookup"><span data-stu-id="bb7fd-136">You can just modify hello <strong>create\_container()</strong> call toopass hello **:public\_access\_level** option:</span></span>

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

<span data-ttu-id="bb7fd-137">Giltiga värden för hello **: offentliga\_åtkomst\_nivå** alternativ är:</span><span class="sxs-lookup"><span data-stu-id="bb7fd-137">Valid values for hello **:public\_access\_level** option are:</span></span>

* <span data-ttu-id="bb7fd-138">**BLOB:** anger offentlig läsbehörighet för blobbar.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-138">**blob:** Specifies public read access for blobs.</span></span> <span data-ttu-id="bb7fd-139">BLOB-data i den här behållaren kan läsas via anonym begäran, men behållardata är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-139">Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="bb7fd-140">Klienter kan inte räkna upp blobbar i behållaren hello via anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-140">Clients cannot enumerate blobs within hello container via anonymous request.</span></span>
* <span data-ttu-id="bb7fd-141">**behållare:** anger fullständig offentlig läsbehörighet för behållaren och blob-data.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-141">**container:** Specifies full public read access for container and blob data.</span></span> <span data-ttu-id="bb7fd-142">Klienter kan räkna upp blobbar i behållaren hello via anonym begäran, men det går inte att räkna upp behållare i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-142">Clients can enumerate blobs within hello container via anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="bb7fd-143">Alternativt, du kan ändra hello offentliga åtkomstnivån för en behållare med hjälp av **ange\_behållare\_acl()** åtkomstnivå för metoden toospecify hello offentliga.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-143">Alternatively, you can modify hello public access level of a container by using **set\_container\_acl()** method toospecify hello public access level.</span></span>

<span data-ttu-id="bb7fd-144">Hej följande kodexempel ändringar hello offentliga åtkomstnivå för**behållare**:</span><span class="sxs-lookup"><span data-stu-id="bb7fd-144">hello following code example changes hello public access level too**container**:</span></span>

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="bb7fd-145">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="bb7fd-145">Upload a blob into a container</span></span>
<span data-ttu-id="bb7fd-146">tooupload innehåll tooa blob, Använd hello **skapa\_block\_blob()** metoden toocreate hello blob, använda en fil eller en sträng som hello innehållet i hello blob.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-146">tooupload content tooa blob, use hello **create\_block\_blob()** method toocreate hello blob, use a file or string as hello content of hello blob.</span></span>

<span data-ttu-id="bb7fd-147">hello följande kod överför hello fil **test.png** som en ny blob med namnet ”-avbildningsbloben” i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-147">hello following code uploads hello file **test.png** as a new blob named "image-blob" in hello container.</span></span>

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="bb7fd-148">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="bb7fd-148">List hello blobs in a container</span></span>
<span data-ttu-id="bb7fd-149">toolist hello behållare använder **list_containers()** metod.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-149">toolist hello containers, use **list_containers()** method.</span></span>
<span data-ttu-id="bb7fd-150">toolist hello blobbar i en behållare använder **lista\_blobs()** metod.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-150">toolist hello blobs within a container, use **list\_blobs()** method.</span></span>

<span data-ttu-id="bb7fd-151">Detta matar ut hello URL: er för alla hello BLOB i alla hello-behållare för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-151">This outputs hello urls of all hello blobs in all hello containers for hello account.</span></span>

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a><span data-ttu-id="bb7fd-152">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="bb7fd-152">Download blobs</span></span>
<span data-ttu-id="bb7fd-153">toodownload blobbar använda hello **hämta\_blob()** metoden tooretrieve hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-153">toodownload blobs, use hello **get\_blob()** method tooretrieve hello contents.</span></span>

<span data-ttu-id="bb7fd-154">hello följande kodexempel visas hur du använder **hämta\_blob()** toodownload hello innehållet i ”avbildningsbloben” och skriver den tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-154">hello following code example demonstrates using **get\_blob()** toodownload hello contents of "image-blob" and write it tooa local file.</span></span>

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a><span data-ttu-id="bb7fd-155">Ta bort en Blobb</span><span class="sxs-lookup"><span data-stu-id="bb7fd-155">Delete a Blob</span></span>
<span data-ttu-id="bb7fd-156">Slutligen toodelete blob använda hello **ta bort\_blob()** metod.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-156">Finally, toodelete a blob, use hello **delete\_blob()** method.</span></span> <span data-ttu-id="bb7fd-157">hello följande kodexempel visar hur toodelete en blob.</span><span class="sxs-lookup"><span data-stu-id="bb7fd-157">hello following code example demonstrates how toodelete a blob.</span></span>

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a><span data-ttu-id="bb7fd-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb7fd-158">Next steps</span></span>
<span data-ttu-id="bb7fd-159">toolearn om mer komplexa lagringsuppgifter följa dessa länkar:</span><span class="sxs-lookup"><span data-stu-id="bb7fd-159">toolearn about more complex storage tasks, follow these links:</span></span>

* [<span data-ttu-id="bb7fd-160">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="bb7fd-160">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="bb7fd-161">[Azure SDK för Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="bb7fd-161">[Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>
* [<span data-ttu-id="bb7fd-162">Överföra data med kommandoradsverktyget Azcopy hello</span><span class="sxs-lookup"><span data-stu-id="bb7fd-162">Transfer data with hello AzCopy Command-Line Utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

