---
title: "aaaGet igång med blob storage och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur tooget igång med Azure Blob storage i ett projekt i Visual Studio ASP.NET Core när du har skapat ett lagringskonto med hjälp av Visual Studio anslutna tjänster"
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
ms.openlocfilehash: 8eedf331896b21658c7b30a68a4391d8d60cd729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="dc0b7-103">Kom igång med Azure Blob storage och Visual Studio anslutna tjänster (ASP.NET kärnor)</span><span class="sxs-lookup"><span data-stu-id="dc0b7-103">Get started with Azure Blob storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="dc0b7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="dc0b7-104">Overview</span></span>
<span data-ttu-id="dc0b7-105">Den här artikeln beskriver hur tooget igång med Azure Blob storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ett projekt för ASP.NET Core hello Visual Studio Lägg till anslutna tjänster dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-105">This article describes how tooget started using Azure Blob storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio Add Connected Services dialog.</span></span>

<span data-ttu-id="dc0b7-106">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data som kan nås från var som helst i hello world via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-106">Azure Blob storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="dc0b7-107">En enda blob kan vara valfri storlek.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-107">A single blob can be any size.</span></span> <span data-ttu-id="dc0b7-108">Blobbar kan vara till exempel bilder, ljud- och bildfiler, rådata och dokumentfiler.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-108">Blobs can be things like images, audio and video files, raw data, and document files.</span></span> <span data-ttu-id="dc0b7-109">Den här artikeln beskriver hur tooget igång med blob storage när du har skapat ett Azure storage-konto med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan i ASP.NET Core-projekt.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-109">This article describes how tooget started with blob storage after you create an Azure storage account by using hello Visual Studio **Add Connected Services** dialog in an ASP.NET Core project.</span></span>

<span data-ttu-id="dc0b7-110">Precis som filer live i mappar, live storage-blobbar i behållare.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-110">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="dc0b7-111">När du har skapat en lagringsenhet, skapar du en eller flera behållare i hello lagring.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-111">After you have created a storage, you create one or more containers in hello storage.</span></span> <span data-ttu-id="dc0b7-112">Till exempel i en lagring som kallas ”klippboken”, kan du skapa behållare i hello lagring som kallas ”bilder” toostore bilder och en annan som heter ”ljud” toostore ljudfiler.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-112">For example, in a storage called "Scrapbook," you can create containers in hello storage called "images" toostore pictures and another called "audio" toostore audio files.</span></span> <span data-ttu-id="dc0b7-113">När du har skapat hello behållare kan du överföra enskilda blob filer toothem.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-113">After you create hello containers, you can upload individual blob files toothem.</span></span> <span data-ttu-id="dc0b7-114">Se [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) mer information om programmässigt manipulering blobbar.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-114">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) for more information on programmatically manipulating blobs.</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="dc0b7-115">Åtkomst till blob-behållare i koden</span><span class="sxs-lookup"><span data-stu-id="dc0b7-115">Access blob containers in code</span></span>
<span data-ttu-id="dc0b7-116">tooprogrammatically komma åt blobar i ASP.NET Core projekt måste tooadd hello följande objekt, om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-116">tooprogrammatically access blobs in ASP.NET Core projects, you need tooadd hello following items, if they're not already present.</span></span>

1. <span data-ttu-id="dc0b7-117">Lägg till hello följande kod namnområde deklarationer toohello upp i en C#-fil som du vill tooprogrammatically åtkomst till Azure storage.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-117">Add hello following code namespace declarations toohello top of any C# file in which you want tooprogrammatically access Azure storage.</span></span>
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. <span data-ttu-id="dc0b7-118">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-118">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="dc0b7-119">Använda följande kod tooget hello anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-119">Use hello following code tooget your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    <span data-ttu-id="dc0b7-120">**Obs:** använda alla hello ovan kod framför hello koden i hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-120">**NOTE:** Use all of hello above code in front of hello code in hello following sections.</span></span>
3. <span data-ttu-id="dc0b7-121">Använd en **CloudBlobClient** objekt tooget en **CloudBlobContainer** referens tooan befintlig behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-121">Use a **CloudBlobClient** object tooget a **CloudBlobContainer** reference tooan existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a><span data-ttu-id="dc0b7-122">Skapa en behållare i koden</span><span class="sxs-lookup"><span data-stu-id="dc0b7-122">Create a container in code</span></span>
<span data-ttu-id="dc0b7-123">Du kan också använda hello **CloudBlobClient** toocreate en behållare i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-123">You can also use hello **CloudBlobClient** toocreate a container in your storage account.</span></span> <span data-ttu-id="dc0b7-124">Allt du behöver toodo är tooadd ett anrop för**CreateIfNotExistsAsync** som hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="dc0b7-124">All you need toodo is tooadd a call too**CreateIfNotExistsAsync** as in hello following code:</span></span>

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference tooa container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="dc0b7-125">**Obs:** hello API: er som utför anrop tooAzure lagring i ASP.NET Core är asynkron.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-125">**NOTE:** hello APIs that perform calls tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="dc0b7-126">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-126">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="dc0b7-127">hello koden nedan förutsätter asynkrona programming metoder som används.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-127">hello code below assumes async programming methods are being used.</span></span>

<span data-ttu-id="dc0b7-128">toomake hello filer i hello behållaren tillgängliga tooeveryone, du kan ange hello behållaren toobe offentlig med hjälp av hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-128">toomake hello files within hello container available tooeveryone, you can set hello container toobe public by using hello following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="dc0b7-129">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="dc0b7-129">Upload a blob into a container</span></span>
<span data-ttu-id="dc0b7-130">tooupload en blob-fil till en behållare och hämta en referens för behållaren och använder det tooget en blobbreferens.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-130">tooupload a blob file into a container, get a container reference and use it tooget a blob reference.</span></span> <span data-ttu-id="dc0b7-131">När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **UploadFromStreamAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-131">After you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStreamAsync** method.</span></span> <span data-ttu-id="dc0b7-132">Den här åtgärden skapar hello blob om den inte redan har gjort det, eller skrivs över om den finns.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-132">This operation creates hello blob if it's not already there, or overwrites it if it does exist.</span></span> <span data-ttu-id="dc0b7-133">följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-133">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

    // Get a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with hello contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="dc0b7-134">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="dc0b7-134">List hello blobs in a container</span></span>
<span data-ttu-id="dc0b7-135">toolist hello blobbar i en behållare först hämta en referens för behållaren.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-135">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="dc0b7-136">Du kan sedan anropa hello behållaren **ListBlobsSegmentedAsync** metoden tooretrieve hello blobbarna och/eller katalogerna i den.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-136">You can then call hello container's **ListBlobsSegmentedAsync** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="dc0b7-137">tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **IListBlobItem**, måste du skicka den tooa **CloudBlockBlob**, **CloudPageBlob**, eller  **CloudBlobDirectory** objekt.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-137">tooaccess hello rich set of properties and methods for a returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="dc0b7-138">Om du inte vet hello blob skriver kan du använda en typ av kontroll toodetermine vilka toocast den.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-138">If you don't know hello blob type, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="dc0b7-139">hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i en behållare.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-139">hello following code demonstrates how tooretrieve and output hello URI of each item in a container.</span></span>

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

<span data-ttu-id="dc0b7-140">Det finns andra sätt toolist hello innehållet i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-140">There are others ways toolist hello contents of a blob container.</span></span> <span data-ttu-id="dc0b7-141">Se [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) för mer information.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-141">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) for more information.</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="dc0b7-142">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="dc0b7-142">Download a blob</span></span>
<span data-ttu-id="dc0b7-143">toodownload blob först hämta en referens toohello blob och sedan anropa hello **DownloadToStreamAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-143">toodownload a blob, first get a reference toohello blob, and then call hello **DownloadToStreamAsync** method.</span></span> <span data-ttu-id="dc0b7-144">hello följande exempel används hello **DownloadToStreamAsync** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara som en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-144">hello following example uses hello **DownloadToStreamAsync** method tootransfer hello blob contents tooa stream object that you can then save as a local file.</span></span>

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save hello blob contents tooa file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

<span data-ttu-id="dc0b7-145">Det finns andra sätt toosave blobbar som filer.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-145">There are other ways toosave blobs as files.</span></span> <span data-ttu-id="dc0b7-146">Se [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) för mer information.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-146">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) for more information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="dc0b7-147">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="dc0b7-147">Delete a blob</span></span>
<span data-ttu-id="dc0b7-148">toodelete blob först hämta en referens toohello blob och sedan anropa hello **DeleteAsync** metod på den.</span><span class="sxs-lookup"><span data-stu-id="dc0b7-148">toodelete a blob, first get a reference toohello blob, and then call hello **DeleteAsync** method on it.</span></span>

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a><span data-ttu-id="dc0b7-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc0b7-149">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

