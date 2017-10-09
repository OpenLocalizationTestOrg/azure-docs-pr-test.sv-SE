---
title: "aaaGet igång med Azure Blob storage (objektlagring) med hjälp av .NET | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: d18a8fc8-97cb-4d37-a408-a6f8107ea8b3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: marsma
ms.openlocfilehash: 9b675ac073e7e901fe1afe54f7bfea12eefbf3ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-using-net"></a><span data-ttu-id="7e76e-103">Komma igång med Azure Blob Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="7e76e-103">Get started with Azure Blob storage using .NET</span></span>

[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

<span data-ttu-id="7e76e-104">Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="7e76e-104">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="7e76e-105">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="7e76e-105">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="7e76e-106">BLOB storage är också hänvisade tooas objektlagring.</span><span class="sxs-lookup"><span data-stu-id="7e76e-106">Blob storage is also referred tooas object storage.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="7e76e-107">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="7e76e-107">About this tutorial</span></span>
<span data-ttu-id="7e76e-108">Den här kursen visar hur toowrite .NET kod för några vanliga scenarier som använder Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7e76e-108">This tutorial shows how toowrite .NET code for some common scenarios using Azure Blob storage.</span></span> <span data-ttu-id="7e76e-109">I kursen beskrivs scenarier där du laddar upp, visar en lista över, laddar ned och tar bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="7e76e-109">Scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

<span data-ttu-id="7e76e-110">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="7e76e-110">**Prerequisites:**</span></span>

* [<span data-ttu-id="7e76e-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e76e-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/)
* [<span data-ttu-id="7e76e-112">Azure Storage-klientbibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="7e76e-112">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="7e76e-113">Azure Configuration Manager för .NET</span><span class="sxs-lookup"><span data-stu-id="7e76e-113">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="7e76e-114">Ett [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="7e76e-114">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="7e76e-115">Fler exempel</span><span class="sxs-lookup"><span data-stu-id="7e76e-115">More samples</span></span>
<span data-ttu-id="7e76e-116">Ytterligare exempel med Blob Storage finns i [Komma igång med Azure Blob Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="7e76e-116">For additional examples using Blob storage, see [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span> <span data-ttu-id="7e76e-117">Du kan ladda ned exempelprogrammet hello och kör den, eller bläddra hello koden på GitHub.</span><span class="sxs-lookup"><span data-stu-id="7e76e-117">You can download hello sample application and run it, or browse hello code on GitHub.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="7e76e-118">Lägga till med hjälp av direktiv</span><span class="sxs-lookup"><span data-stu-id="7e76e-118">Add using directives</span></span>
<span data-ttu-id="7e76e-119">Lägg till följande hello **med** direktiven toohello överkant hello `Program.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="7e76e-119">Add hello following **using** directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="7e76e-120">Parsa anslutningssträngen för hello</span><span class="sxs-lookup"><span data-stu-id="7e76e-120">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-blob-service-client"></a><span data-ttu-id="7e76e-121">Skapa hello Blob-klienten</span><span class="sxs-lookup"><span data-stu-id="7e76e-121">Create hello Blob service client</span></span>
<span data-ttu-id="7e76e-122">Hej **CloudBlobClient** klassen kan du tooretrieve behållare och blobbar som lagras i Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7e76e-122">hello **CloudBlobClient** class enables you tooretrieve containers and blobs stored in Blob storage.</span></span> <span data-ttu-id="7e76e-123">Här är ett sätt toocreate hello-klienten:</span><span class="sxs-lookup"><span data-stu-id="7e76e-123">Here's one way toocreate hello service client:</span></span>

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
<span data-ttu-id="7e76e-124">Nu är du redo toowrite kod som läser data från och skriver tooBlob datalagring.</span><span class="sxs-lookup"><span data-stu-id="7e76e-124">Now you are ready toowrite code that reads data from and writes data tooBlob storage.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="7e76e-125">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="7e76e-125">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="7e76e-126">Det här exemplet illustrerar hur toocreate en behållare om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="7e76e-126">This example shows how toocreate a container if it does not already exist:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create hello container if it doesn't already exist.
container.CreateIfNotExists();
```

<span data-ttu-id="7e76e-127">Som standard är hello nya behållaren privat, vilket innebär att du måste ange lagring få åtkomst till viktiga toodownload blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="7e76e-127">By default, hello new container is private, meaning that you must specify your storage access key toodownload blobs from this container.</span></span> <span data-ttu-id="7e76e-128">Om du vill toomake hello filer i hello behållaren tillgängliga tooeveryone, kan du ange hello behållaren toobe offentliga med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="7e76e-128">If you want toomake hello files within hello container available tooeveryone, you can set hello container toobe public using hello following code:</span></span>

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

<span data-ttu-id="7e76e-129">Vem som helst på hello Internet kan se blobbar i en offentlig behållare.</span><span class="sxs-lookup"><span data-stu-id="7e76e-129">Anyone on hello Internet can see blobs in a public container.</span></span> <span data-ttu-id="7e76e-130">Du kan ändra eller ta bort dem bara om du har hello lämpligt konto åtkomst till nyckeln eller en signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7e76e-130">However, you can modify or delete them only if you have hello appropriate account access key or a shared access signature.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="7e76e-131">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="7e76e-131">Upload a blob into a container</span></span>
<span data-ttu-id="7e76e-132">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="7e76e-132">Azure Blob Storage supports block blobs and page blobs.</span></span>  <span data-ttu-id="7e76e-133">I de flesta fall är blockblob hello rekommenderade typen toouse.</span><span class="sxs-lookup"><span data-stu-id="7e76e-133">In most cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="7e76e-134">tooupload en fil tooa blockblob, hämta en referens för behållaren och använder det tooget en blockblobben.</span><span class="sxs-lookup"><span data-stu-id="7e76e-134">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="7e76e-135">När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **UploadFromStream** metod.</span><span class="sxs-lookup"><span data-stu-id="7e76e-135">Once you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStream** method.</span></span> <span data-ttu-id="7e76e-136">Den här åtgärden skapar hello blob om det inte fanns tidigare, eller skrivs över om den finns.</span><span class="sxs-lookup"><span data-stu-id="7e76e-136">This operation creates hello blob if it didn't previously exist, or overwrites it if it does exist.</span></span>

<span data-ttu-id="7e76e-137">följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="7e76e-137">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite hello "myblob" blob with contents from a local file.
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    blockBlob.UploadFromStream(fileStream);
}
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="7e76e-138">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="7e76e-138">List hello blobs in a container</span></span>
<span data-ttu-id="7e76e-139">toolist hello blobbar i en behållare först hämta en referens för behållaren.</span><span class="sxs-lookup"><span data-stu-id="7e76e-139">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="7e76e-140">Du kan sedan använda hello behållaren **ListBlobs** metoden tooretrieve hello blobbarna och/eller katalogerna i den.</span><span class="sxs-lookup"><span data-stu-id="7e76e-140">You can then use hello container's **ListBlobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="7e76e-141">tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **IListBlobItem**, måste du skicka den tooa **CloudBlockBlob**, **CloudPageBlob**, eller  **CloudBlobDirectory** objekt.</span><span class="sxs-lookup"><span data-stu-id="7e76e-141">tooaccess hello  rich set of properties and methods for a returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="7e76e-142">Om hello typen är okänd, kan du använda en typ av kontroll toodetermine vilka toocast den.</span><span class="sxs-lookup"><span data-stu-id="7e76e-142">If hello type is unknown, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="7e76e-143">hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i hello _foton_ behållare:</span><span class="sxs-lookup"><span data-stu-id="7e76e-143">hello following code demonstrates how tooretrieve and output hello URI of each item in hello _photos_ container:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("photos");

// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, false))
{
    if (item.GetType() == typeof(CloudBlockBlob))
    {
        CloudBlockBlob blob = (CloudBlockBlob)item;

        Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

    }
    else if (item.GetType() == typeof(CloudPageBlob))
    {
        CloudPageBlob pageBlob = (CloudPageBlob)item;

        Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

    }
    else if (item.GetType() == typeof(CloudBlobDirectory))
    {
        CloudBlobDirectory directory = (CloudBlobDirectory)item;

        Console.WriteLine("Directory: {0}", directory.Uri);
    }
}
```

<span data-ttu-id="7e76e-144">Genom att ta med information om sökvägen i blobbnamnen kan du skapa en virtuell katalogstruktur som du kan ordna och bläddra i på samma sätt som ett traditionellt filsystem.</span><span class="sxs-lookup"><span data-stu-id="7e76e-144">By including path information in blob names, you can create a virtual directory structure you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="7e76e-145">hello katalogstrukturen är virtuell endast--hello endast tillgängliga resurserna i Blob storage är behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="7e76e-145">hello directory structure is virtual only--hello only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="7e76e-146">Dock hello klientbiblioteket tillhandahåller en **CloudBlobDirectory** objekt toorefer tooa virtuell katalog och förenkla processen hello arbetet med blobbar som är ordnade på det här sättet.</span><span class="sxs-lookup"><span data-stu-id="7e76e-146">However, hello storage client library offers a **CloudBlobDirectory** object toorefer tooa virtual directory and simplify hello process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="7e76e-147">Tänk dig följande uppsättning blockblobbar i en behållare med namnet hello *foton*:</span><span class="sxs-lookup"><span data-stu-id="7e76e-147">For example, consider hello following set of block blobs in a container named *photos*:</span></span>

```
photo1.jpg
2010/architecture/description.txt
2010/architecture/photo3.jpg
2010/architecture/photo4.jpg
2011/architecture/photo5.jpg
2011/architecture/photo6.jpg
2011/architecture/description.txt
2011/photo7.jpg
```

<span data-ttu-id="7e76e-148">När du anropar **ListBlobs** på hello *foton* behållare (som hello som föregående kodfragment IT-avdelning), returneras en hierarkiskt ordnad lista.</span><span class="sxs-lookup"><span data-stu-id="7e76e-148">When you call **ListBlobs** on hello *photos* container (as in hello preceding code snippet), a hierarchical listing is returned.</span></span> <span data-ttu-id="7e76e-149">Den innehåller både **CloudBlobDirectory** och **CloudBlockBlob** objekt, som representerar hello kataloger respektive blobbar i hello-behållaren.</span><span class="sxs-lookup"><span data-stu-id="7e76e-149">It contains both **CloudBlobDirectory** and **CloudBlockBlob** objects, representing hello directories and blobs in hello container, respectively.</span></span> <span data-ttu-id="7e76e-150">hello resultatet ser ut så:</span><span class="sxs-lookup"><span data-stu-id="7e76e-150">hello resulting output looks like:</span></span>

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

<span data-ttu-id="7e76e-151">Du kan ange hello **UseFlatBlobListing** parametern för hello **ListBlobs** metod för att **SANT**.</span><span class="sxs-lookup"><span data-stu-id="7e76e-151">Optionally, you can set hello **UseFlatBlobListing** parameter of hello **ListBlobs** method to **true**.</span></span> <span data-ttu-id="7e76e-152">I detta fall returneras varje blobb i behållaren hello som en **CloudBlockBlob** objekt.</span><span class="sxs-lookup"><span data-stu-id="7e76e-152">In this case, every blob in hello container is returned as a **CloudBlockBlob** object.</span></span> <span data-ttu-id="7e76e-153">Hej anrop för**ListBlobs** tooreturn en platt lista ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="7e76e-153">hello call too**ListBlobs** tooreturn a flat listing looks like this:</span></span>

```csharp
// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

<span data-ttu-id="7e76e-154">och hello resultatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="7e76e-154">and hello results look like this:</span></span>

```
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a><span data-ttu-id="7e76e-155">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="7e76e-155">Download blobs</span></span>
<span data-ttu-id="7e76e-156">toodownload blobbar först hämta en blobbreferens och sedan anropa hello **DownloadToStream** metod.</span><span class="sxs-lookup"><span data-stu-id="7e76e-156">toodownload blobs, first retrieve a blob reference and then call hello **DownloadToStream** method.</span></span> <span data-ttu-id="7e76e-157">hello följande exempel används hello **DownloadToStream** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="7e76e-157">hello following example uses hello **DownloadToStream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save blob contents tooa file.
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    blockBlob.DownloadToStream(fileStream);
}
```

<span data-ttu-id="7e76e-158">Du kan också använda hello **DownloadToStream** metoden toodownload hello innehållet i en blob som en textsträng.</span><span class="sxs-lookup"><span data-stu-id="7e76e-158">You can also use hello **DownloadToStream** method toodownload hello contents of a blob as a text string.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt"
CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

string text;
using (var memoryStream = new MemoryStream())
{
    blockBlob2.DownloadToStream(memoryStream);
    text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
}
```

## <a name="delete-blobs"></a><span data-ttu-id="7e76e-159">Ta bort blobbar</span><span class="sxs-lookup"><span data-stu-id="7e76e-159">Delete blobs</span></span>
<span data-ttu-id="7e76e-160">toodelete blob först hämta en blobbreferens och anropar sedan den **ta bort** metod på den.</span><span class="sxs-lookup"><span data-stu-id="7e76e-160">toodelete a blob, first get a blob reference and then call the **Delete** method on it.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete hello blob.
blockBlob.Delete();
```

## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="7e76e-161">Visa en lista över blobbar på sidor asynkront</span><span class="sxs-lookup"><span data-stu-id="7e76e-161">List blobs in pages asynchronously</span></span>
<span data-ttu-id="7e76e-162">Om du visar ett stort antal blobbar eller om du vill toocontrol hello antalet resultat som returneras i samma åtgärd, kan du visa blobbar på resultatsidor.</span><span class="sxs-lookup"><span data-stu-id="7e76e-162">If you are listing a large number of blobs, or you want toocontrol hello number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="7e76e-163">Det här exemplet visar hur tooreturn resultat på sidor asynkront, så att körningen inte blockeras medan du väntar på tooreturn ett stort antal resultat.</span><span class="sxs-lookup"><span data-stu-id="7e76e-163">This example shows how tooreturn results in pages asynchronously, so that execution is not blocked while waiting tooreturn a large set of results.</span></span>

<span data-ttu-id="7e76e-164">Det här exemplet illustrerar en platt blob lista, men du kan också utföra en hierarkiskt ordnad lista genom att ange hello _useFlatBlobListing_ parametern för hello **ListBlobsSegmentedAsync** metoden too_false_.</span><span class="sxs-lookup"><span data-stu-id="7e76e-164">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting hello _useFlatBlobListing_ parameter of hello **ListBlobsSegmentedAsync** method too_false_.</span></span>

<span data-ttu-id="7e76e-165">Eftersom hello exempelmetoden anropar en asynkron metod, den måste föregås hello _asynkrona_ nyckelord och det måste returnera en **aktivitet** objekt.</span><span class="sxs-lookup"><span data-stu-id="7e76e-165">Because hello sample method calls an asynchronous method, it must be prefaced with hello _async_ keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="7e76e-166">Hej await nyckelord som angetts för hello **ListBlobsSegmentedAsync** metoden pausar körningen av hello exempelmetoden tills listuppgiften hello har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7e76e-166">hello await keyword specified for hello **ListBlobsSegmentedAsync** method suspends execution of hello sample method until hello listing task completes.</span></span>

```csharp
async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
{
    //List blobs toohello console window, with paging.
    Console.WriteLine("List blobs in pages:");

    int i = 0;
    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
    //When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
    do
    {
        //This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
        if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
        foreach (var blobItem in resultSegment.Results)
        {
            Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
        }
        Console.WriteLine();

        //Get hello continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

## <a name="writing-tooan-append-blob"></a><span data-ttu-id="7e76e-167">Skrivning tooan lägga till blob</span><span class="sxs-lookup"><span data-stu-id="7e76e-167">Writing tooan append blob</span></span>
<span data-ttu-id="7e76e-168">En tilläggsblobb är optimerad för tilläggsåtgärder, t.ex loggning.</span><span class="sxs-lookup"><span data-stu-id="7e76e-168">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="7e76e-169">Som en blockblobb består en tilläggsblobb av block, men när du lägger till en ny tilläggsblobb för block tooan det är alltid tillagda toohello slutet av hello-blob.</span><span class="sxs-lookup"><span data-stu-id="7e76e-169">Like a block blob, an append blob is comprised of blocks, but when you add a new block tooan append blob, it is always appended toohello end of hello blob.</span></span> <span data-ttu-id="7e76e-170">Du kan inte uppdatera eller ta bort ett befintligt block i en tilläggsblobb.</span><span class="sxs-lookup"><span data-stu-id="7e76e-170">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="7e76e-171">hello block ID för en tilläggsblobb exponeras inte eftersom de är för en blockblob.</span><span class="sxs-lookup"><span data-stu-id="7e76e-171">hello block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="7e76e-172">Varje block i en tilläggsblobb kan ha olika storlek, upp tooa högst 4 MB och en tilläggsblobb kan innehålla högst 50 000 block.</span><span class="sxs-lookup"><span data-stu-id="7e76e-172">Each block in an append blob can be a different size, up tooa maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="7e76e-173">hello maximal storlek på en tilläggsblobb är därför lite över 195 GB (4 MB × 50 000 block).</span><span class="sxs-lookup"><span data-stu-id="7e76e-173">hello maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="7e76e-174">hello exemplet nedan skapar en ny tilläggsblobb och lägger till vissa data tooit, simulera en enkel loggningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="7e76e-174">hello example below creates a new append blob and appends some data tooit, simulating a simple logging operation.</span></span>

```csharp
//Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

//Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

//Create hello container if it does not already exist.
container.CreateIfNotExists();

//Get a reference tooan append blob.
CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

//Create hello append blob. Note that if hello blob already exists, hello CreateOrReplace() method will overwrite it.
//You can check whether hello blob exists tooavoid overwriting it by using CloudAppendBlob.Exists().
appendBlob.CreateOrReplace();

int numBlocks = 10;

//Generate an array of random bytes.
Random rnd = new Random();
byte[] bytes = new byte[numBlocks];
rnd.NextBytes(bytes);

//Simulate a logging operation by writing text data and byte data toohello end of hello append blob.
for (int i = 0; i < numBlocks; i++)
{
    appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
        DateTime.UtcNow, bytes[i], Environment.NewLine));
}

//Read hello append blob toohello console window.
Console.WriteLine(appendBlob.DownloadText());
```

<span data-ttu-id="7e76e-175">Se [förstå Blockblobbar, Sidblobbar och Tilläggsblobbar](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) för mer information om hello skillnaderna mellan hello tre typer av blobbar.</span><span class="sxs-lookup"><span data-stu-id="7e76e-175">See [Understanding Block Blobs, Page Blobs, and Append Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) for more information about hello differences between hello three types of blobs.</span></span>

## <a name="managing-security-for-blobs"></a><span data-ttu-id="7e76e-176">Hantera säkerheten för blobbar</span><span class="sxs-lookup"><span data-stu-id="7e76e-176">Managing security for blobs</span></span>
<span data-ttu-id="7e76e-177">Standard skydda Azure Storage dina data genom att begränsa åtkomst toohello kontoägaren, som innehar hello åtkomstnycklarna för kontot.</span><span class="sxs-lookup"><span data-stu-id="7e76e-177">By default, Azure Storage keeps your data secure by limiting access toohello account owner, who is in possession of hello account access keys.</span></span> <span data-ttu-id="7e76e-178">När du behöver tooshare blob-data i ditt lagringskonto är det viktigt toodo så utan att kompromissa med hello säkerheten för åtkomstnycklarna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="7e76e-178">When you need tooshare blob data in your storage account, it is important toodo so without compromising hello security of your account access keys.</span></span> <span data-ttu-id="7e76e-179">Dessutom kan du kryptera blob data tooensure som de är skyddade över hello överföring och i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7e76e-179">Additionally, you can encrypt blob data tooensure that it is secure going over hello wire and in Azure Storage.</span></span>

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-tooblob-data"></a><span data-ttu-id="7e76e-180">Hur du styr åtkomst till tooblob data</span><span class="sxs-lookup"><span data-stu-id="7e76e-180">Controlling access tooblob data</span></span>
<span data-ttu-id="7e76e-181">Hello blob-data i ditt lagringskonto är tillgänglig endast toostorage kontoägaren som standard.</span><span class="sxs-lookup"><span data-stu-id="7e76e-181">By default, hello blob data in your storage account is accessible only toostorage account owner.</span></span> <span data-ttu-id="7e76e-182">Autentisering av förfrågningar mot Blob storage krävs åtkomstnyckeln hello som standard.</span><span class="sxs-lookup"><span data-stu-id="7e76e-182">Authenticating requests against Blob storage requires hello account access key by default.</span></span> <span data-ttu-id="7e76e-183">Dock gärna toomake vissa blob-data tillgängliga tooother användare.</span><span class="sxs-lookup"><span data-stu-id="7e76e-183">However, you may wish toomake certain blob data available tooother users.</span></span> <span data-ttu-id="7e76e-184">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="7e76e-184">You have two options:</span></span>

* <span data-ttu-id="7e76e-185">**Anonym åtkomst:** Du kan göra en behållare eller dess blobbar offentligt tillgängliga för anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7e76e-185">**Anonymous access:** You can make a container or its blobs publicly available for anonymous access.</span></span> <span data-ttu-id="7e76e-186">Se [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="7e76e-186">See [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="7e76e-187">**Signaturer för delad åtkomst:** du kan ge klienterna en signatur för delad åtkomst (SAS), som ger delegerad åtkomst tooa resurs i ditt lagringskonto med behörigheter som du anger och under den period som du anger.</span><span class="sxs-lookup"><span data-stu-id="7e76e-187">**Shared access signatures:** You can provide clients with a shared access signature (SAS), which provides delegated access tooa resource in your storage account, with permissions that you specify and over an interval that you specify.</span></span> <span data-ttu-id="7e76e-188">Mer information finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="7e76e-188">See [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) for more information.</span></span>

### <a name="encrypting-blob-data"></a><span data-ttu-id="7e76e-189">Kryptera blobbdata</span><span class="sxs-lookup"><span data-stu-id="7e76e-189">Encrypting blob data</span></span>
<span data-ttu-id="7e76e-190">Azure Storage har stöd för kryptering av blobbdata både på hello klient och server hello:</span><span class="sxs-lookup"><span data-stu-id="7e76e-190">Azure Storage supports encrypting blob data both at hello client and on hello server:</span></span>

* <span data-ttu-id="7e76e-191">**Kryptering på klientsidan:** hello Storage-klientbibliotek för .NET har stöd för kryptering av data i klientprogram före överföringen tooAzure lagring och dekryptera data vid hämtning av toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="7e76e-191">**Client-side encryption:** hello Storage Client Library for .NET supports encrypting data within client applications before uploading tooAzure Storage, and decrypting data while downloading toohello client.</span></span> <span data-ttu-id="7e76e-192">hello biblioteket stöder även integrering med Azure Key Vault storage-konto för hantering av nycklar.</span><span class="sxs-lookup"><span data-stu-id="7e76e-192">hello library also supports integration with Azure Key Vault for storage account key management.</span></span> <span data-ttu-id="7e76e-193">Mer information finns i [Kryptering på klientsidan med .NET för Microsoft Azure Storage](storage-client-side-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="7e76e-193">See [Client-Side Encryption with .NET for Microsoft Azure Storage](storage-client-side-encryption.md) for more information.</span></span> <span data-ttu-id="7e76e-194">Se även [Självstudier: Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="7e76e-194">Also see [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span></span>
* <span data-ttu-id="7e76e-195">**Kryptering på serversidan**: Nu stöder Azure Storage kryptering på serversidan.</span><span class="sxs-lookup"><span data-stu-id="7e76e-195">**Server-side encryption**: Azure Storage now supports server-side encryption.</span></span> <span data-ttu-id="7e76e-196">Mer information finns i [Azure Storage Service-kryptering av vilande data (förhandsgranskning)](storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="7e76e-196">See [Azure Storage Service Encryption for Data at Rest (Preview)](storage-service-encryption.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e76e-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e76e-197">Next steps</span></span>
<span data-ttu-id="7e76e-198">Nu när du har lärt dig grunderna hello i Blob storage följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="7e76e-198">Now that you've learned hello basics of Blob storage, follow these links toolearn more.</span></span>

### <a name="microsoft-azure-storage-explorer"></a><span data-ttu-id="7e76e-199">Microsoft Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="7e76e-199">Microsoft Azure Storage Explorer</span></span>
* <span data-ttu-id="7e76e-200">[Microsoft Azure Storage Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="7e76e-200">[Microsoft Azure Storage Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

### <a name="blob-storage-samples"></a><span data-ttu-id="7e76e-201">Exempel på Blob Storage (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="7e76e-201">Blob storage samples</span></span>
* [<span data-ttu-id="7e76e-202">Komma igång med Azure Blob Storage i .NET</span><span class="sxs-lookup"><span data-stu-id="7e76e-202">Getting Started with Azure Blob Storage in .NET</span></span>](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a><span data-ttu-id="7e76e-203">Referens för Blob Storage</span><span class="sxs-lookup"><span data-stu-id="7e76e-203">Blob storage reference</span></span>
* [<span data-ttu-id="7e76e-204">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="7e76e-204">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [<span data-ttu-id="7e76e-205">REST API-referens</span><span class="sxs-lookup"><span data-stu-id="7e76e-205">REST API reference</span></span>](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a><span data-ttu-id="7e76e-206">Begreppsorienterade guider</span><span class="sxs-lookup"><span data-stu-id="7e76e-206">Conceptual guides</span></span>
* [<span data-ttu-id="7e76e-207">Överföra data med hello kommandoradsverktyget azcopy</span><span class="sxs-lookup"><span data-stu-id="7e76e-207">Transfer data with hello AzCopy command-line utility</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="7e76e-208">Komma igång med File Storage för .NET</span><span class="sxs-lookup"><span data-stu-id="7e76e-208">Get started with File storage for .NET</span></span>](storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="7e76e-209">Hur toouse Azure blob storage med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="7e76e-209">How toouse Azure blob storage with hello WebJobs SDK</span></span>](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
