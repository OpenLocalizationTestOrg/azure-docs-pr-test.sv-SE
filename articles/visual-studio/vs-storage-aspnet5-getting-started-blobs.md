---
title: "Kom igång med blob storage och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur du kommer igång med Azure Blob storage i ett projekt i Visual Studio ASP.NET Core när du har skapat ett lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 2e8060b44c395ad7c24e7778c0ef65148a3a45de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="7ff22-103">Kom igång med Azure Blob storage och Visual Studio anslutna tjänster (ASP.NET kärnor)</span><span class="sxs-lookup"><span data-stu-id="7ff22-103">Get started with Azure Blob storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="7ff22-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7ff22-104">Overview</span></span>
<span data-ttu-id="7ff22-105">Den här artikeln beskriver hur du kommer igång med Azure Blob storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ASP.NET Core-projekt med hjälp av dialogrutan Visual Studio Lägg till anslutna tjänster.</span><span class="sxs-lookup"><span data-stu-id="7ff22-105">This article describes how to get started using Azure Blob storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio Add Connected Services dialog.</span></span>

<span data-ttu-id="7ff22-106">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data som kan nås från var som helst i världen via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ff22-106">Azure Blob storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="7ff22-107">En enda blob kan vara valfri storlek.</span><span class="sxs-lookup"><span data-stu-id="7ff22-107">A single blob can be any size.</span></span> <span data-ttu-id="7ff22-108">Blobbar kan vara till exempel bilder, ljud- och bildfiler, rådata och dokumentfiler.</span><span class="sxs-lookup"><span data-stu-id="7ff22-108">Blobs can be things like images, audio and video files, raw data, and document files.</span></span> <span data-ttu-id="7ff22-109">Den här artikeln beskriver hur du kommer igång med blob storage när du har skapat ett Azure storage-konto med hjälp av Visual Studio **Lägg till anslutna tjänster** dialogrutan i ASP.NET Core-projekt.</span><span class="sxs-lookup"><span data-stu-id="7ff22-109">This article describes how to get started with blob storage after you create an Azure storage account by using the Visual Studio **Add Connected Services** dialog in an ASP.NET Core project.</span></span>

<span data-ttu-id="7ff22-110">Precis som filer live i mappar, live storage-blobbar i behållare.</span><span class="sxs-lookup"><span data-stu-id="7ff22-110">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="7ff22-111">När du har skapat en lagringsenhet, skapar du en eller flera behållare i lagringen.</span><span class="sxs-lookup"><span data-stu-id="7ff22-111">After you have created a storage, you create one or more containers in the storage.</span></span> <span data-ttu-id="7ff22-112">Till exempel i en lagring som kallas ”klippboken”, kan du skapa behållare i lagringen som kallas ”bilder” för att lagra bilder och en annan som heter ”ljud” för att lagra ljudfiler.</span><span class="sxs-lookup"><span data-stu-id="7ff22-112">For example, in a storage called "Scrapbook," you can create containers in the storage called "images" to store pictures and another called "audio" to store audio files.</span></span> <span data-ttu-id="7ff22-113">Du kan ladda upp enskilda blob-filer till dem när du har skapat behållarna.</span><span class="sxs-lookup"><span data-stu-id="7ff22-113">After you create the containers, you can upload individual blob files to them.</span></span> <span data-ttu-id="7ff22-114">Se [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) mer information om programmässigt manipulering blobbar.</span><span class="sxs-lookup"><span data-stu-id="7ff22-114">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) for more information on programmatically manipulating blobs.</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="7ff22-115">Åtkomst till blob-behållare i koden</span><span class="sxs-lookup"><span data-stu-id="7ff22-115">Access blob containers in code</span></span>
<span data-ttu-id="7ff22-116">Om du vill komma åt blobar i ASP.NET Core projekt, måste du lägga till följande objekt om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="7ff22-116">To programmatically access blobs in ASP.NET Core projects, you need to add the following items, if they're not already present.</span></span>

1. <span data-ttu-id="7ff22-117">Lägg till följande kod namnrymdsdeklarationer överst i en C#-fil som du vill komma åt Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="7ff22-117">Add the following code namespace declarations to the top of any C# file in which you want to programmatically access Azure storage.</span></span>
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. <span data-ttu-id="7ff22-118">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="7ff22-118">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="7ff22-119">Använda följande kod för att hämta dina anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7ff22-119">Use the following code to get your storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    <span data-ttu-id="7ff22-120">**Obs:** använda alla koden ovan framför koden i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7ff22-120">**NOTE:** Use all of the above code in front of the code in the following sections.</span></span>
3. <span data-ttu-id="7ff22-121">Använd en **CloudBlobClient** objekt för att hämta en **CloudBlobContainer** referens till en befintlig behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7ff22-121">Use a **CloudBlobClient** object to get a **CloudBlobContainer** reference to an existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a><span data-ttu-id="7ff22-122">Skapa en behållare i koden</span><span class="sxs-lookup"><span data-stu-id="7ff22-122">Create a container in code</span></span>
<span data-ttu-id="7ff22-123">Du kan också använda den **CloudBlobClient** att skapa en behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7ff22-123">You can also use the **CloudBlobClient** to create a container in your storage account.</span></span> <span data-ttu-id="7ff22-124">Allt du behöver göra är att lägga till ett anrop till **CreateIfNotExistsAsync** enligt följande:</span><span class="sxs-lookup"><span data-stu-id="7ff22-124">All you need to do is to add a call to **CreateIfNotExistsAsync** as in the following code:</span></span>

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="7ff22-125">**Obs:** API: er som utför anrop till Azure-lagring i ASP.NET Core är asynkron.</span><span class="sxs-lookup"><span data-stu-id="7ff22-125">**NOTE:** The APIs that perform calls to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="7ff22-126">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="7ff22-126">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="7ff22-127">Koden nedan förutsätter asynkrona programming metoder som används.</span><span class="sxs-lookup"><span data-stu-id="7ff22-127">The code below assumes async programming methods are being used.</span></span>

<span data-ttu-id="7ff22-128">Du kan ange att behållaren ska vara offentlig med hjälp av följande kod för att göra filerna i behållaren ska vara tillgänglig för alla.</span><span class="sxs-lookup"><span data-stu-id="7ff22-128">To make the files within the container available to everyone, you can set the container to be public by using the following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="7ff22-129">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="7ff22-129">Upload a blob into a container</span></span>
<span data-ttu-id="7ff22-130">Hämta en referens för behållaren och använda den för att hämta en blobbreferens om du vill överföra en blob-fil till en behållare.</span><span class="sxs-lookup"><span data-stu-id="7ff22-130">To upload a blob file into a container, get a container reference and use it to get a blob reference.</span></span> <span data-ttu-id="7ff22-131">När du har en blobbreferens kan du ladda upp en dataström till den genom att anropa den **UploadFromStreamAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="7ff22-131">After you have a blob reference, you can upload any stream of data to it by calling the **UploadFromStreamAsync** method.</span></span> <span data-ttu-id="7ff22-132">Den här åtgärden skapas blobben om den inte redan har gjort det, eller skrivs över om den finns.</span><span class="sxs-lookup"><span data-stu-id="7ff22-132">This operation creates the blob if it's not already there, or overwrites it if it does exist.</span></span> <span data-ttu-id="7ff22-133">Följande exempel visar hur du laddar upp en blobb till en behållare och förutsätter att behållaren redan hade skapats.</span><span class="sxs-lookup"><span data-stu-id="7ff22-133">The following example shows how to upload a blob into a container and assumes that the container was already created.</span></span>

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="7ff22-134">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="7ff22-134">List the blobs in a container</span></span>
<span data-ttu-id="7ff22-135">Om du vill visa blobbar i en behållare börjar du med att hämta en referens för behållaren.</span><span class="sxs-lookup"><span data-stu-id="7ff22-135">To list the blobs in a container, first get a container reference.</span></span> <span data-ttu-id="7ff22-136">Du kan sedan anropa behållarens **ListBlobsSegmentedAsync** metod för att hämta blobbarna och/eller katalogerna i den.</span><span class="sxs-lookup"><span data-stu-id="7ff22-136">You can then call the container's **ListBlobsSegmentedAsync** method to retrieve the blobs and/or directories within it.</span></span> <span data-ttu-id="7ff22-137">För att komma åt den omfattande uppsättningen med egenskaper och metoder för en returnerad **IListBlobItem**-metod måste du skicka den till ett **CloudBlockBlob**-, **CloudPageBlob**- eller **CloudBlobDirectory**-objekt.</span><span class="sxs-lookup"><span data-stu-id="7ff22-137">To access the rich set of properties and methods for a returned **IListBlobItem**, you must cast it to a **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="7ff22-138">Om du inte vet blob-datatyp, kan du använda en typkontroll för att fastställa som du vill skicka den till.</span><span class="sxs-lookup"><span data-stu-id="7ff22-138">If you don't know the blob type, you can use a type check to determine which to cast it to.</span></span> <span data-ttu-id="7ff22-139">Följande kod visar hur du hämtar och returnerar URI: N för varje objekt i en behållare.</span><span class="sxs-lookup"><span data-stu-id="7ff22-139">The following code demonstrates how to retrieve and output the URI of each item in a container.</span></span>

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

<span data-ttu-id="7ff22-140">Det finns andra sätt att visa innehållet i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="7ff22-140">There are others ways to list the contents of a blob container.</span></span> <span data-ttu-id="7ff22-141">Se [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) för mer information.</span><span class="sxs-lookup"><span data-stu-id="7ff22-141">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) for more information.</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="7ff22-142">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="7ff22-142">Download a blob</span></span>
<span data-ttu-id="7ff22-143">Om du vill hämta en blob först hämta en referens till blobben och sedan anropa den **DownloadToStreamAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="7ff22-143">To download a blob, first get a reference to the blob, and then call the **DownloadToStreamAsync** method.</span></span> <span data-ttu-id="7ff22-144">I följande exempel används den **DownloadToStreamAsync** metod för att överföra blobbinnehållet till ett stream-objekt som du sedan kan spara som en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="7ff22-144">The following example uses the **DownloadToStreamAsync** method to transfer the blob contents to a stream object that you can then save as a local file.</span></span>

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

<span data-ttu-id="7ff22-145">Det finns andra sätt att spara blobbar som filer.</span><span class="sxs-lookup"><span data-stu-id="7ff22-145">There are other ways to save blobs as files.</span></span> <span data-ttu-id="7ff22-146">Se [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) för mer information.</span><span class="sxs-lookup"><span data-stu-id="7ff22-146">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) for more information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="7ff22-147">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="7ff22-147">Delete a blob</span></span>
<span data-ttu-id="7ff22-148">Om du vill ta bort en blobb först hämta en referens till blobben och sedan anropa den **DeleteAsync** metod på den.</span><span class="sxs-lookup"><span data-stu-id="7ff22-148">To delete a blob, first get a reference to the blob, and then call the **DeleteAsync** method on it.</span></span>

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a><span data-ttu-id="7ff22-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ff22-149">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

