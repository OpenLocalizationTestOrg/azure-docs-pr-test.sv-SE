---
title: "Använda Blob storage (objektlagring) från Ruby | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
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
ms.openlocfilehash: 7f7d0c52b2b50a360711477e8e0eafc07ddcf374
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blob-storage-from-ruby"></a><span data-ttu-id="4258e-103">Använda Blob Storage från Ruby</span><span class="sxs-lookup"><span data-stu-id="4258e-103">How to use Blob storage from Ruby</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="4258e-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4258e-104">Overview</span></span>
<span data-ttu-id="4258e-105">Azure Blob Storage är en tjänst som lagrar ostrukturerade data i molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="4258e-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="4258e-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="4258e-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="4258e-107">Blob Storage kallas även för objektlagring.</span><span class="sxs-lookup"><span data-stu-id="4258e-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="4258e-108">Den här guiden visar hur du utför vanliga scenarier med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="4258e-108">This guide will show you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="4258e-109">Exemplen är skrivna med hjälp av Ruby-API.</span><span class="sxs-lookup"><span data-stu-id="4258e-109">The samples are written using the Ruby API.</span></span> <span data-ttu-id="4258e-110">Scenarier som tas upp inkluderar **överför, lista, hämtar** och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="4258e-110">The scenarios covered include **uploading, listing, downloading,** and **deleting** blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="4258e-111">Skapa ett Ruby-program</span><span class="sxs-lookup"><span data-stu-id="4258e-111">Create a Ruby application</span></span>
<span data-ttu-id="4258e-112">Skapa ett Ruby program.</span><span class="sxs-lookup"><span data-stu-id="4258e-112">Create a Ruby application.</span></span> <span data-ttu-id="4258e-113">Instruktioner finns i [Ruby på spår webbprogram på en Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span><span class="sxs-lookup"><span data-stu-id="4258e-113">For instructions, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="4258e-114">Konfigurera ditt program åtkomst till lagring</span><span class="sxs-lookup"><span data-stu-id="4258e-114">Configure your application to access Storage</span></span>
<span data-ttu-id="4258e-115">Om du vill använda Azure Storage, måste du hämtar och använder Ruby azure-paketet, som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4258e-115">To use Azure Storage, you need to download and use the Ruby azure package, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="4258e-116">Använda RubyGems för att hämta paketet</span><span class="sxs-lookup"><span data-stu-id="4258e-116">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="4258e-117">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="4258e-117">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="4258e-118">Skriv ”symbolen installera azure” i kommandofönstret att installera symbolen och beroenden.</span><span class="sxs-lookup"><span data-stu-id="4258e-118">Type "gem install azure" in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="4258e-119">Importera paketet</span><span class="sxs-lookup"><span data-stu-id="4258e-119">Import the package</span></span>
<span data-ttu-id="4258e-120">Med hjälp av valfri textredigerare, lägger du till följande upp i filen Ruby där du tänker använda lagring:</span><span class="sxs-lookup"><span data-stu-id="4258e-120">Using your favorite text editor, add the following to the top of the Ruby file where you intend to use storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="4258e-121">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="4258e-121">Set up an Azure Storage Connection</span></span>
<span data-ttu-id="4258e-122">Azure-modulen läses miljövariablerna **AZURE\_lagring\_konto** och **AZURE\_lagring\_ACCESS_KEY** information som krävs för att ansluta till Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4258e-122">The azure module will read the environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="4258e-123">Om de här miljövariablerna inte har angetts måste du ange kontoinformation innan du använder **Azure::Blob::BlobService** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="4258e-123">If these environment variables are not set, you must specify the account information before using **Azure::Blob::BlobService** with the following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="4258e-124">Du kan hämta dessa värden från en klassiska eller Resource Manager storage-konto i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="4258e-124">To obtain these values from a classic or Resource Manager storage account in the Azure portal:</span></span>

1. <span data-ttu-id="4258e-125">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4258e-125">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4258e-126">Navigera till storage-konto som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="4258e-126">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="4258e-127">I bladet inställningar till höger klickar du på **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="4258e-127">In the Settings blade on the right, click **Access Keys**.</span></span>
4. <span data-ttu-id="4258e-128">I åtkomst bladet nycklar som visas, visas den åtkomstnyckel 1 och åtkomstnyckel 2.</span><span class="sxs-lookup"><span data-stu-id="4258e-128">In the Access keys blade that appears, you'll see the access key 1 and access key 2.</span></span> <span data-ttu-id="4258e-129">Du kan använda någon av dessa.</span><span class="sxs-lookup"><span data-stu-id="4258e-129">You can use either of these.</span></span>
5. <span data-ttu-id="4258e-130">Klicka på Kopiera-ikonen för att kopiera nyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="4258e-130">Click the copy icon to copy the key to the clipboard.</span></span>

<span data-ttu-id="4258e-131">Du kan hämta dessa värden från ett klassiska storage-konto i den klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="4258e-131">To obtain these values from a classic storage account in the classic Azure portal:</span></span>

1. <span data-ttu-id="4258e-132">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4258e-132">Log in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="4258e-133">Navigera till storage-konto som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="4258e-133">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="4258e-134">Klicka på **hantera ÅTKOMSTNYCKLAR** längst ned i navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4258e-134">Click **MANAGE ACCESS KEYS** at the bottom of the navigation pane.</span></span>
4. <span data-ttu-id="4258e-135">I popup-fönstret visas lagringskontonamn, primärnyckeln och sekundära åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="4258e-135">In the pop-up dialog, you'll see the storage account name, primary access key and secondary access key.</span></span> <span data-ttu-id="4258e-136">Du kan använda antingen den primära eller sekundära en för åtkomst till nyckeln.</span><span class="sxs-lookup"><span data-stu-id="4258e-136">For access key, you can use either the primary one or the secondary one.</span></span>
5. <span data-ttu-id="4258e-137">Klicka på Kopiera-ikonen för att kopiera nyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="4258e-137">Click the copy icon to copy the key to the clipboard.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="4258e-138">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="4258e-138">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="4258e-139">Den **Azure::Blob::BlobService** objekt kan du arbeta med behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="4258e-139">The **Azure::Blob::BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="4258e-140">Du kan skapa en behållare med den **skapa\_container()** metod.</span><span class="sxs-lookup"><span data-stu-id="4258e-140">To create a container, use the **create\_container()** method.</span></span>

<span data-ttu-id="4258e-141">Följande exempel skapar en behållare eller skriver ut felet om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="4258e-141">The following code example creates a container or prints the error if there is any.</span></span>

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

<span data-ttu-id="4258e-142">Om du vill göra filerna i behållaren offentliga kan du ange behållarens behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4258e-142">If you want to make the files in the container public, you can set the container's permissions.</span></span>

<span data-ttu-id="4258e-143">Du kan bara ändra det <strong>skapa\_container()</strong> anrop för att skicka den **: offentliga\_åtkomst\_nivå** alternativ:</span><span class="sxs-lookup"><span data-stu-id="4258e-143">You can just modify the <strong>create\_container()</strong> call to pass the **:public\_access\_level** option:</span></span>

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

<span data-ttu-id="4258e-144">Giltiga värden för den **: offentliga\_åtkomst\_nivå** alternativ är:</span><span class="sxs-lookup"><span data-stu-id="4258e-144">Valid values for the **:public\_access\_level** option are:</span></span>

* <span data-ttu-id="4258e-145">**BLOB:** anger offentlig läsbehörighet för blobbar.</span><span class="sxs-lookup"><span data-stu-id="4258e-145">**blob:** Specifies public read access for blobs.</span></span> <span data-ttu-id="4258e-146">BLOB-data i den här behållaren kan läsas via anonym begäran, men behållardata är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="4258e-146">Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="4258e-147">Klienter kan inte räkna upp blobbar i behållaren via anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="4258e-147">Clients cannot enumerate blobs within the container via anonymous request.</span></span>
* <span data-ttu-id="4258e-148">**behållare:** anger fullständig offentlig läsbehörighet för behållaren och blob-data.</span><span class="sxs-lookup"><span data-stu-id="4258e-148">**container:** Specifies full public read access for container and blob data.</span></span> <span data-ttu-id="4258e-149">Klienter kan räkna upp blobbar i behållaren via anonym begäran, men det går inte att räkna upp behållare i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="4258e-149">Clients can enumerate blobs within the container via anonymous request, but cannot enumerate containers within the storage account.</span></span>

<span data-ttu-id="4258e-150">Du kan också ändra offentliga åtkomstnivån för en behållare med hjälp av **ange\_behållare\_acl()** metod för att ange nivån offentlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4258e-150">Alternatively, you can modify the public access level of a container by using **set\_container\_acl()** method to specify the public access level.</span></span>

<span data-ttu-id="4258e-151">Följande exempel ändras offentliga åtkomstnivå till **behållare**:</span><span class="sxs-lookup"><span data-stu-id="4258e-151">The following code example changes the public access level to **container**:</span></span>

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="4258e-152">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="4258e-152">Upload a blob into a container</span></span>
<span data-ttu-id="4258e-153">Om du vill överföra innehåll till en blobb, använder den **skapa\_block\_blob()** metoden för att skapa blob, använder en fil eller en sträng som innehåll i blob.</span><span class="sxs-lookup"><span data-stu-id="4258e-153">To upload content to a blob, use the **create\_block\_blob()** method to create the blob, use a file or string as the content of the blob.</span></span>

<span data-ttu-id="4258e-154">Följande kod överför filen **test.png** som en ny blob med namnet ”-avbildningsbloben” i behållaren.</span><span class="sxs-lookup"><span data-stu-id="4258e-154">The following code uploads the file **test.png** as a new blob named "image-blob" in the container.</span></span>

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="4258e-155">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="4258e-155">List the blobs in a container</span></span>
<span data-ttu-id="4258e-156">Om du vill visa behållarna som använder **list_containers()** metod.</span><span class="sxs-lookup"><span data-stu-id="4258e-156">To list the containers, use **list_containers()** method.</span></span>
<span data-ttu-id="4258e-157">Om du vill visa blobbar i en behållare använder **lista\_blobs()** metod.</span><span class="sxs-lookup"><span data-stu-id="4258e-157">To list the blobs within a container, use **list\_blobs()** method.</span></span>

<span data-ttu-id="4258e-158">Detta anger URL: er för alla blobbar i alla behållare för kontot.</span><span class="sxs-lookup"><span data-stu-id="4258e-158">This outputs the urls of all the blobs in all the containers for the account.</span></span>

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a><span data-ttu-id="4258e-159">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="4258e-159">Download blobs</span></span>
<span data-ttu-id="4258e-160">Om du vill ladda ned blobbar använda av **hämta\_blob()** metod för att hämta innehållet.</span><span class="sxs-lookup"><span data-stu-id="4258e-160">To download blobs, use the **get\_blob()** method to retrieve the contents.</span></span>

<span data-ttu-id="4258e-161">I följande kodexempel visas hur du använder **hämta\_blob()** att hämta innehållet i ”avbildningsbloben” och skriver den till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="4258e-161">The following code example demonstrates using **get\_blob()** to download the contents of "image-blob" and write it to a local file.</span></span>

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a><span data-ttu-id="4258e-162">Ta bort en Blobb</span><span class="sxs-lookup"><span data-stu-id="4258e-162">Delete a Blob</span></span>
<span data-ttu-id="4258e-163">Slutligen, om du vill ta bort en blobb, använda den **ta bort\_blob()** metod.</span><span class="sxs-lookup"><span data-stu-id="4258e-163">Finally, to delete a blob, use the **delete\_blob()** method.</span></span> <span data-ttu-id="4258e-164">Följande kodexempel visar hur du tar bort en blob.</span><span class="sxs-lookup"><span data-stu-id="4258e-164">The following code example demonstrates how to delete a blob.</span></span>

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a><span data-ttu-id="4258e-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4258e-165">Next steps</span></span>
<span data-ttu-id="4258e-166">Mer information om mer komplexa lagringsuppgifter, kan du följa dessa länkar:</span><span class="sxs-lookup"><span data-stu-id="4258e-166">To learn about more complex storage tasks, follow these links:</span></span>

* [<span data-ttu-id="4258e-167">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="4258e-167">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="4258e-168">[Azure SDK för Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="4258e-168">[Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>
* [<span data-ttu-id="4258e-169">Överföra data med kommandoradsverktyget AzCopy</span><span class="sxs-lookup"><span data-stu-id="4258e-169">Transfer data with the AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)

