---
title: "Komma igång med Azure Blob Storage (objektlagring) med hjälp av .NET | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
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
ms.openlocfilehash: 70c7d6a5e1b9aa9a13481893e0baa56538be097c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-using-net"></a><span data-ttu-id="79e2a-103">Komma igång med Azure Blob Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="79e2a-103">Get started with Azure Blob storage using .NET</span></span>

[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

<span data-ttu-id="79e2a-104">Azure Blob Storage är en tjänst som lagrar ostrukturerade data i molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="79e2a-104">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="79e2a-105">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="79e2a-105">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="79e2a-106">Blob Storage kallas även för objektlagring.</span><span class="sxs-lookup"><span data-stu-id="79e2a-106">Blob storage is also referred to as object storage.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="79e2a-107">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="79e2a-107">About this tutorial</span></span>
<span data-ttu-id="79e2a-108">I den här kursen lär du dig hur du skriver .NET-kod för några vanliga scenarier med hjälp av Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="79e2a-108">This tutorial shows how to write .NET code for some common scenarios using Azure Blob storage.</span></span> <span data-ttu-id="79e2a-109">I kursen beskrivs scenarier där du laddar upp, visar en lista över, laddar ned och tar bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="79e2a-109">Scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

<span data-ttu-id="79e2a-110">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="79e2a-110">**Prerequisites:**</span></span>

* [<span data-ttu-id="79e2a-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79e2a-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/)
* [<span data-ttu-id="79e2a-112">Azure Storage-klientbibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="79e2a-112">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="79e2a-113">Azure Configuration Manager för .NET</span><span class="sxs-lookup"><span data-stu-id="79e2a-113">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="79e2a-114">Ett [Azure Storage-konto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="79e2a-114">An [Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="79e2a-115">Fler exempel</span><span class="sxs-lookup"><span data-stu-id="79e2a-115">More samples</span></span>
<span data-ttu-id="79e2a-116">Ytterligare exempel med Blob Storage finns i [Komma igång med Azure Blob Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="79e2a-116">For additional examples using Blob storage, see [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span> <span data-ttu-id="79e2a-117">Du kan ladda ned exempelprogrammet och köra det eller bläddra i koden på GitHub.</span><span class="sxs-lookup"><span data-stu-id="79e2a-117">You can download the sample application and run it, or browse the code on GitHub.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="79e2a-118">Lägga till med hjälp av direktiv</span><span class="sxs-lookup"><span data-stu-id="79e2a-118">Add using directives</span></span>
<span data-ttu-id="79e2a-119">Lägg till följande **using**-direktiv längst upp i filen `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="79e2a-119">Add the following **using** directives to the top of the `Program.cs` file:</span></span>

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-the-connection-string"></a><span data-ttu-id="79e2a-120">Parsa anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="79e2a-120">Parse the connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a><span data-ttu-id="79e2a-121">Skapa klient för Blob-tjänst</span><span class="sxs-lookup"><span data-stu-id="79e2a-121">Create the Blob service client</span></span>
<span data-ttu-id="79e2a-122">Med **CloudBlobClient** kan du hämta behållare och blobbar som lagras i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="79e2a-122">The **CloudBlobClient** class enables you to retrieve containers and blobs stored in Blob storage.</span></span> <span data-ttu-id="79e2a-123">Här är ett sätt att skapa tjänstklienten:</span><span class="sxs-lookup"><span data-stu-id="79e2a-123">Here's one way to create the service client:</span></span>

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
<span data-ttu-id="79e2a-124">Nu är det dags att skriva kod som läser data från och skriver data till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="79e2a-124">Now you are ready to write code that reads data from and writes data to Blob storage.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="79e2a-125">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="79e2a-125">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="79e2a-126">Det här exemplet visas hur du skapar en behållare om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="79e2a-126">This example shows how to create a container if it does not already exist:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();
```

<span data-ttu-id="79e2a-127">Som standard är den nya behållaren privat, vilket innebär att du måste ange din lagringsåtkomstnyckel för att ladda ned blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="79e2a-127">By default, the new container is private, meaning that you must specify your storage access key to download blobs from this container.</span></span> <span data-ttu-id="79e2a-128">Om du vill göra filerna i behållaren tillgängliga för alla kan du ange att behållaren ska vara offentlig med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="79e2a-128">If you want to make the files within the container available to everyone, you can set the container to be public using the following code:</span></span>

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

<span data-ttu-id="79e2a-129">Alla på Internet kan se blobbar i en offentlig behållare.</span><span class="sxs-lookup"><span data-stu-id="79e2a-129">Anyone on the Internet can see blobs in a public container.</span></span> <span data-ttu-id="79e2a-130">Du kan dock bara ändra eller ta bort dem om du har rätt kontoåtkomstnyckel eller signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="79e2a-130">However, you can modify or delete them only if you have the appropriate account access key or a shared access signature.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="79e2a-131">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="79e2a-131">Upload a blob into a container</span></span>
<span data-ttu-id="79e2a-132">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="79e2a-132">Azure Blob Storage supports block blobs and page blobs.</span></span>  <span data-ttu-id="79e2a-133">I de flesta fall är blockblob den rekommenderade typen.</span><span class="sxs-lookup"><span data-stu-id="79e2a-133">In most cases, block blob is the recommended type to use.</span></span>

<span data-ttu-id="79e2a-134">Om du vill ladda upp en fil till en blockblobb hämtar du en referens för behållaren och använder den för att hämta en referens för blockblobben.</span><span class="sxs-lookup"><span data-stu-id="79e2a-134">To upload a file to a block blob, get a container reference and use it to get a block blob reference.</span></span> <span data-ttu-id="79e2a-135">När du har en blobbreferens kan du ladda upp en dataström till den genom att anropa metoden **UploadFromStream**.</span><span class="sxs-lookup"><span data-stu-id="79e2a-135">Once you have a blob reference, you can upload any stream of data to it by calling the **UploadFromStream** method.</span></span> <span data-ttu-id="79e2a-136">Den här åtgärden skapar blobben om den inte redan fanns, eller skriver över den om den finns.</span><span class="sxs-lookup"><span data-stu-id="79e2a-136">This operation creates the blob if it didn't previously exist, or overwrites it if it does exist.</span></span>

<span data-ttu-id="79e2a-137">Följande exempel visar hur du laddar upp en blobb till en behållare och förutsätter att behållaren redan hade skapats.</span><span class="sxs-lookup"><span data-stu-id="79e2a-137">The following example shows how to upload a blob into a container and assumes that the container was already created.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite the "myblob" blob with contents from a local file.
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    blockBlob.UploadFromStream(fileStream);
}
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="79e2a-138">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="79e2a-138">List the blobs in a container</span></span>
<span data-ttu-id="79e2a-139">Om du vill visa blobbar i en behållare börjar du med att hämta en referens för behållaren.</span><span class="sxs-lookup"><span data-stu-id="79e2a-139">To list the blobs in a container, first get a container reference.</span></span> <span data-ttu-id="79e2a-140">Sedan kan du använda behållarens **ListBlobs**-metod för att hämta blobbarna och/eller katalogerna i den.</span><span class="sxs-lookup"><span data-stu-id="79e2a-140">You can then use the container's **ListBlobs** method to retrieve the blobs and/or directories within it.</span></span> <span data-ttu-id="79e2a-141">För att komma åt den omfattande uppsättningen med egenskaper och metoder för en returnerad **IListBlobItem**-metod måste du skicka den till ett **CloudBlockBlob**-, **CloudPageBlob**- eller **CloudBlobDirectory**-objekt.</span><span class="sxs-lookup"><span data-stu-id="79e2a-141">To access the  rich set of properties and methods for a returned **IListBlobItem**, you must cast it to a **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="79e2a-142">Om typen är okänd kan du använda en typkontroll för att avgöra till vilket objekt den ska skickas.</span><span class="sxs-lookup"><span data-stu-id="79e2a-142">If the type is unknown, you can use a type check to determine which to cast it to.</span></span> <span data-ttu-id="79e2a-143">Följande kod visar hur du hämtar och returnerar URI:n för varje objekt i _photos_-behållaren:</span><span class="sxs-lookup"><span data-stu-id="79e2a-143">The following code demonstrates how to retrieve and output the URI of each item in the _photos_ container:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("photos");

// Loop over items within the container and output the length and URI.
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

<span data-ttu-id="79e2a-144">Genom att ta med information om sökvägen i blobbnamnen kan du skapa en virtuell katalogstruktur som du kan ordna och bläddra i på samma sätt som ett traditionellt filsystem.</span><span class="sxs-lookup"><span data-stu-id="79e2a-144">By including path information in blob names, you can create a virtual directory structure you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="79e2a-145">Katalogstrukturen är bara virtuell – de enda tillgängliga resurserna i Blob Storage är behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="79e2a-145">The directory structure is virtual only--the only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="79e2a-146">Klientbiblioteket tillhandahåller dock ett **CloudBlobDirectory**-objekt för att referera till en virtuell katalog och förenkla arbetet med blobbar som är ordnade på det här sättet.</span><span class="sxs-lookup"><span data-stu-id="79e2a-146">However, the storage client library offers a **CloudBlobDirectory** object to refer to a virtual directory and simplify the process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="79e2a-147">Titta exempelvis på följande uppsättning blockblobbar i en behållare med namnet *photos*:</span><span class="sxs-lookup"><span data-stu-id="79e2a-147">For example, consider the following set of block blobs in a container named *photos*:</span></span>

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

<span data-ttu-id="79e2a-148">När du anropar **ListBlobs** i *photos*-behållaren (som i föregående kodfragment) returneras en hierarkisk lista.</span><span class="sxs-lookup"><span data-stu-id="79e2a-148">When you call **ListBlobs** on the *photos* container (as in the preceding code snippet), a hierarchical listing is returned.</span></span> <span data-ttu-id="79e2a-149">Den innehåller både **CloudBlobDirectory**  och **CloudBlockBlob**objekt, som representerar kataloger respektive blobbar i behållaren.</span><span class="sxs-lookup"><span data-stu-id="79e2a-149">It contains both **CloudBlobDirectory** and **CloudBlockBlob** objects, representing the directories and blobs in the container, respectively.</span></span> <span data-ttu-id="79e2a-150">Resultatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="79e2a-150">The resulting output looks like:</span></span>

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

<span data-ttu-id="79e2a-151">Om du vill kan du ange parametern **UseFlatBlobListing** för **ListBlobs**-metoden till **True**.</span><span class="sxs-lookup"><span data-stu-id="79e2a-151">Optionally, you can set the **UseFlatBlobListing** parameter of the **ListBlobs** method to **true**.</span></span> <span data-ttu-id="79e2a-152">I detta fall returneras varje blobb i behållaren som ett **CloudBlockBlob**-objekt.</span><span class="sxs-lookup"><span data-stu-id="79e2a-152">In this case, every blob in the container is returned as a **CloudBlockBlob** object.</span></span> <span data-ttu-id="79e2a-153">Anropet till **ListBlobs** för att returnera en platt lista ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="79e2a-153">The call to **ListBlobs** to return a flat listing looks like this:</span></span>

```csharp
// Loop over items within the container and output the length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

<span data-ttu-id="79e2a-154">och resultatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="79e2a-154">and the results look like this:</span></span>

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

## <a name="download-blobs"></a><span data-ttu-id="79e2a-155">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="79e2a-155">Download blobs</span></span>
<span data-ttu-id="79e2a-156">Om du vill ladda ned blobbar börjar du med att hämta en blobbreferens och anropar sedan **DownloadToStream**-metoden.</span><span class="sxs-lookup"><span data-stu-id="79e2a-156">To download blobs, first retrieve a blob reference and then call the **DownloadToStream** method.</span></span> <span data-ttu-id="79e2a-157">I följande exempel används metoden **DownloadToStream** för att överföra blobbinnehållet till ett dataströmsobjekt som du sedan kan spara till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="79e2a-157">The following example uses the **DownloadToStream** method to transfer the blob contents to a stream object that you can then persist to a local file.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save blob contents to a file.
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    blockBlob.DownloadToStream(fileStream);
}
```

<span data-ttu-id="79e2a-158">Du kan också använda metoden **DownloadToStream** för att ladda ned innehållet i en blobb som en textsträng.</span><span class="sxs-lookup"><span data-stu-id="79e2a-158">You can also use the **DownloadToStream** method to download the contents of a blob as a text string.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "myblob.txt"
CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

string text;
using (var memoryStream = new MemoryStream())
{
    blockBlob2.DownloadToStream(memoryStream);
    text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
}
```

## <a name="delete-blobs"></a><span data-ttu-id="79e2a-159">Ta bort blobbar</span><span class="sxs-lookup"><span data-stu-id="79e2a-159">Delete blobs</span></span>
<span data-ttu-id="79e2a-160">Om du vill ta bort en blobb börjar du med att hämta en blobbreferens och anropar sedan metoden **Delete** för den.</span><span class="sxs-lookup"><span data-stu-id="79e2a-160">To delete a blob, first get a blob reference and then call the **Delete** method on it.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
blockBlob.Delete();
```

## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="79e2a-161">Visa en lista över blobbar på sidor asynkront</span><span class="sxs-lookup"><span data-stu-id="79e2a-161">List blobs in pages asynchronously</span></span>
<span data-ttu-id="79e2a-162">Om du visar ett stort antal blobbar, eller om du vill styra hur många resultat som returneras i samma åtgärd, kan du visa blobbar på resultatsidor.</span><span class="sxs-lookup"><span data-stu-id="79e2a-162">If you are listing a large number of blobs, or you want to control the number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="79e2a-163">Det här exemplet illustrerar hur du returnerar resultat på sidor asynkront, så att körningen inte blockeras när ett stort antal resultat väntar på att returneras.</span><span class="sxs-lookup"><span data-stu-id="79e2a-163">This example shows how to return results in pages asynchronously, so that execution is not blocked while waiting to return a large set of results.</span></span>

<span data-ttu-id="79e2a-164">I det här exemplet returneras en platt blobblista, men du kan också visa en hierarkiskt ordnad lista genom att ange _useFlatBlobListing_-parametern för **ListBlobsSegmentedAsync**-metoden till _false_.</span><span class="sxs-lookup"><span data-stu-id="79e2a-164">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting the _useFlatBlobListing_ parameter of the **ListBlobsSegmentedAsync** method to _false_.</span></span>

<span data-ttu-id="79e2a-165">Eftersom exempelmetoden anropar en asynkron metod måste den föregås av nyckelordet _async_ och returnera ett **Task**-objekt.</span><span class="sxs-lookup"><span data-stu-id="79e2a-165">Because the sample method calls an asynchronous method, it must be prefaced with the _async_ keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="79e2a-166">Nyckelordet await som angetts för **ListBlobsSegmentedAsync**-metoden pausar körningen av exempelmetoden tills listuppgiften har slutförts.</span><span class="sxs-lookup"><span data-stu-id="79e2a-166">The await keyword specified for the **ListBlobsSegmentedAsync** method suspends execution of the sample method until the listing task completes.</span></span>

```csharp
async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
{
    //List blobs to the console window, with paging.
    Console.WriteLine("List blobs in pages:");

    int i = 0;
    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
    //When the continuation token is null, the last page has been returned and execution can exit the loop.
    do
    {
        //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
        if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
        foreach (var blobItem in resultSegment.Results)
        {
            Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
        }
        Console.WriteLine();

        //Get the continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

## <a name="writing-to-an-append-blob"></a><span data-ttu-id="79e2a-167">Skriva till en tilläggsblobb</span><span class="sxs-lookup"><span data-stu-id="79e2a-167">Writing to an append blob</span></span>
<span data-ttu-id="79e2a-168">En tilläggsblobb är optimerad för tilläggsåtgärder, t.ex loggning.</span><span class="sxs-lookup"><span data-stu-id="79e2a-168">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="79e2a-169">Precis som en blockblobb består en tilläggsblobb av block, men när du lägger till ett nytt block till en tilläggsblobb läggs det alltid till sist i blobben.</span><span class="sxs-lookup"><span data-stu-id="79e2a-169">Like a block blob, an append blob is comprised of blocks, but when you add a new block to an append blob, it is always appended to the end of the blob.</span></span> <span data-ttu-id="79e2a-170">Du kan inte uppdatera eller ta bort ett befintligt block i en tilläggsblobb.</span><span class="sxs-lookup"><span data-stu-id="79e2a-170">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="79e2a-171">En tilläggsblobbs block-ID:n exponeras inte som de gör för en blockblobb.</span><span class="sxs-lookup"><span data-stu-id="79e2a-171">The block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="79e2a-172">Blocken i en tilläggsblobb kan ha olika storlek, upp till högst 4 MB, och en tilläggsblobb kan innehålla högst 50 000 block.</span><span class="sxs-lookup"><span data-stu-id="79e2a-172">Each block in an append blob can be a different size, up to a maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="79e2a-173">Den största storleken på en tilläggsblobb är alltså strax över 195 GB (4 MB × 50 000 block).</span><span class="sxs-lookup"><span data-stu-id="79e2a-173">The maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="79e2a-174">I exemplet nedan skapar vi en ny tilläggsblobb och lägger till vissa data i den för att simulera en enkel loggningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="79e2a-174">The example below creates a new append blob and appends some data to it, simulating a simple logging operation.</span></span>

```csharp
//Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

//Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

//Create the container if it does not already exist.
container.CreateIfNotExists();

//Get a reference to an append blob.
CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

//Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
//You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
appendBlob.CreateOrReplace();

int numBlocks = 10;

//Generate an array of random bytes.
Random rnd = new Random();
byte[] bytes = new byte[numBlocks];
rnd.NextBytes(bytes);

//Simulate a logging operation by writing text data and byte data to the end of the append blob.
for (int i = 0; i < numBlocks; i++)
{
    appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
        DateTime.UtcNow, bytes[i], Environment.NewLine));
}

//Read the append blob to the console window.
Console.WriteLine(appendBlob.DownloadText());
```

<span data-ttu-id="79e2a-175">Mer information om skillnaderna mellan de tre typerna av blobbar finns i [Förstå blockblobbar, sidblobbar och tilläggsblobbar](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span><span class="sxs-lookup"><span data-stu-id="79e2a-175">See [Understanding Block Blobs, Page Blobs, and Append Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) for more information about the differences between the three types of blobs.</span></span>

## <a name="managing-security-for-blobs"></a><span data-ttu-id="79e2a-176">Hantera säkerheten för blobbar</span><span class="sxs-lookup"><span data-stu-id="79e2a-176">Managing security for blobs</span></span>
<span data-ttu-id="79e2a-177">Som standard skyddar Azure Storage dina data genom att begränsa åtkomsten till endast kontoägaren, som har tillgång till åtkomstnycklarna för kontot.</span><span class="sxs-lookup"><span data-stu-id="79e2a-177">By default, Azure Storage keeps your data secure by limiting access to the account owner, who is in possession of the account access keys.</span></span> <span data-ttu-id="79e2a-178">När du behöver dela blobbdata i ditt lagringskonto är det viktigt att du gör det utan att äventyra åtkomstnycklarnas säkerhet.</span><span class="sxs-lookup"><span data-stu-id="79e2a-178">When you need to share blob data in your storage account, it is important to do so without compromising the security of your account access keys.</span></span> <span data-ttu-id="79e2a-179">Du kan också kryptera blobbdata så att de är skyddade under kabelöverföringar och i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="79e2a-179">Additionally, you can encrypt blob data to ensure that it is secure going over the wire and in Azure Storage.</span></span>

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a><span data-ttu-id="79e2a-180">Kontrollera åtkomsten till blobbdata</span><span class="sxs-lookup"><span data-stu-id="79e2a-180">Controlling access to blob data</span></span>
<span data-ttu-id="79e2a-181">Som standard är blobbdata i ett lagringskonto endast tillgängliga för lagringskontoägaren.</span><span class="sxs-lookup"><span data-stu-id="79e2a-181">By default, the blob data in your storage account is accessible only to storage account owner.</span></span> <span data-ttu-id="79e2a-182">För autentisering av förfrågningar mot Blob Storage krävs åtkomstnyckeln för kontot som standard.</span><span class="sxs-lookup"><span data-stu-id="79e2a-182">Authenticating requests against Blob storage requires the account access key by default.</span></span> <span data-ttu-id="79e2a-183">Dock kanske du vill göra vissa blobbdata tillgängliga för andra användare.</span><span class="sxs-lookup"><span data-stu-id="79e2a-183">However, you may wish to make certain blob data available to other users.</span></span> <span data-ttu-id="79e2a-184">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="79e2a-184">You have two options:</span></span>

* <span data-ttu-id="79e2a-185">**Anonym åtkomst:** Du kan göra en behållare eller dess blobbar offentligt tillgängliga för anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="79e2a-185">**Anonymous access:** You can make a container or its blobs publicly available for anonymous access.</span></span> <span data-ttu-id="79e2a-186">Mer information finns i [Hantera anonym läsbehörighet till behållare och blobbar](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="79e2a-186">See [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="79e2a-187">**Signaturer för delad åtkomst:** Du kan ge klienterna en signatur för delad åtkomst (SAS), som ger delegerad åtkomst till en resurs i ditt lagringskonto, med behörigheter som du anger och under den period som du anger.</span><span class="sxs-lookup"><span data-stu-id="79e2a-187">**Shared access signatures:** You can provide clients with a shared access signature (SAS), which provides delegated access to a resource in your storage account, with permissions that you specify and over an interval that you specify.</span></span> <span data-ttu-id="79e2a-188">Mer information finns i [Använda signaturer för delad åtkomst (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="79e2a-188">See [Using Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span>

### <a name="encrypting-blob-data"></a><span data-ttu-id="79e2a-189">Kryptera blobbdata</span><span class="sxs-lookup"><span data-stu-id="79e2a-189">Encrypting blob data</span></span>
<span data-ttu-id="79e2a-190">Azure Storage har stöd för kryptering av blobbdata både på klienten och på servern:</span><span class="sxs-lookup"><span data-stu-id="79e2a-190">Azure Storage supports encrypting blob data both at the client and on the server:</span></span>

* <span data-ttu-id="79e2a-191">**Kryptering på klientsidan:** Storage-klientbiblioteket för .NET har stöd för kryptering av data i klientprogram före överföringen till Azure Storage, och dekryptering av data under nedladdningen till klienten.</span><span class="sxs-lookup"><span data-stu-id="79e2a-191">**Client-side encryption:** The Storage Client Library for .NET supports encrypting data within client applications before uploading to Azure Storage, and decrypting data while downloading to the client.</span></span> <span data-ttu-id="79e2a-192">Biblioteket stöder även integrering med Azure Key Vault för hantering av nycklar för lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="79e2a-192">The library also supports integration with Azure Key Vault for storage account key management.</span></span> <span data-ttu-id="79e2a-193">Mer information finns i [Kryptering på klientsidan med .NET för Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="79e2a-193">See [Client-Side Encryption with .NET for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span> <span data-ttu-id="79e2a-194">Se även [Självstudier: Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="79e2a-194">Also see [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span></span>
* <span data-ttu-id="79e2a-195">**Kryptering på serversidan**: Nu stöder Azure Storage kryptering på serversidan.</span><span class="sxs-lookup"><span data-stu-id="79e2a-195">**Server-side encryption**: Azure Storage now supports server-side encryption.</span></span> <span data-ttu-id="79e2a-196">Mer information finns i [Azure Storage Service-kryptering av vilande data (förhandsgranskning)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="79e2a-196">See [Azure Storage Service Encryption for Data at Rest (Preview)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="79e2a-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79e2a-197">Next steps</span></span>
<span data-ttu-id="79e2a-198">Nu när du har lärt dig grunderna om Blob Storage kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="79e2a-198">Now that you've learned the basics of Blob storage, follow these links to learn more.</span></span>

### <a name="microsoft-azure-storage-explorer"></a><span data-ttu-id="79e2a-199">Microsoft Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="79e2a-199">Microsoft Azure Storage Explorer</span></span>
* <span data-ttu-id="79e2a-200">[Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) är en kostnadsfri, fristående app från Microsoft som gör det möjligt att arbeta visuellt med Azure Storage-data i Windows, Mac OS och Linux.</span><span class="sxs-lookup"><span data-stu-id="79e2a-200">[Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

### <a name="blob-storage-samples"></a><span data-ttu-id="79e2a-201">Exempel på Blob Storage (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="79e2a-201">Blob storage samples</span></span>
* [<span data-ttu-id="79e2a-202">Komma igång med Azure Blob Storage i .NET</span><span class="sxs-lookup"><span data-stu-id="79e2a-202">Getting Started with Azure Blob Storage in .NET</span></span>](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a><span data-ttu-id="79e2a-203">Referens för Blob Storage</span><span class="sxs-lookup"><span data-stu-id="79e2a-203">Blob storage reference</span></span>
* [<span data-ttu-id="79e2a-204">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="79e2a-204">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [<span data-ttu-id="79e2a-205">REST API-referens</span><span class="sxs-lookup"><span data-stu-id="79e2a-205">REST API reference</span></span>](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a><span data-ttu-id="79e2a-206">Begreppsorienterade guider</span><span class="sxs-lookup"><span data-stu-id="79e2a-206">Conceptual guides</span></span>
* [<span data-ttu-id="79e2a-207">Överföra data med kommandoradsverktyget AzCopy</span><span class="sxs-lookup"><span data-stu-id="79e2a-207">Transfer data with the AzCopy command-line utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="79e2a-208">Komma igång med File Storage för .NET</span><span class="sxs-lookup"><span data-stu-id="79e2a-208">Get started with File storage for .NET</span></span>](../files/storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="79e2a-209">Använda Azure Blob Storage med WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="79e2a-209">How to use Azure blob storage with the WebJobs SDK</span></span>](../../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
