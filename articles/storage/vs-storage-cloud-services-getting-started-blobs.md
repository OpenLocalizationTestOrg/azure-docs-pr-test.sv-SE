---
title: "aaaGet igång med blob storage och Visual Studio anslutna tjänster (molntjänster) | Microsoft Docs"
description: "Hur tooget igång med Azure Blob storage i ett molntjänstprojekt i Visual Studio när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 62fb7fcff0a90008859ebe23755f13ef0555e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="4d451-103">Kom igång med Azure Blob Storage och Visual Studio anslutna tjänster (cloud services-projekt)</span><span class="sxs-lookup"><span data-stu-id="4d451-103">Get started with Azure Blob Storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="4d451-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4d451-104">Overview</span></span>
<span data-ttu-id="4d451-105">Den här artikeln beskriver hur tooget igång med Azure Blob Storage när du har skapat eller refererar till ett Azure Storage-konto med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan i ett Visual Studio-moln services-projekt.</span><span class="sxs-lookup"><span data-stu-id="4d451-105">This article describes how tooget started with Azure Blob Storage after you created or referenced an Azure Storage account by using hello Visual Studio **Add Connected Services** dialog in a Visual Studio cloud services project.</span></span> <span data-ttu-id="4d451-106">Får du lära dig hur tooaccess och skapa blob-behållare och hur tooperform vanliga uppgifter som överföra, lista och ladda ned blobbar.</span><span class="sxs-lookup"><span data-stu-id="4d451-106">We'll show you how tooaccess and create blob containers, and how tooperform common tasks like uploading, listing, and downloading blobs.</span></span> <span data-ttu-id="4d451-107">hello prover är skrivna i C\# och använda hello [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d451-107">hello samples are written in C\# and use hello [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="4d451-108">Azure Blob Storage är en tjänst för att lagra stora mängder Ostrukturerade data som kan nås från var som helst i hello world via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4d451-108">Azure Blob Storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="4d451-109">En enda blob kan vara valfri storlek.</span><span class="sxs-lookup"><span data-stu-id="4d451-109">A single blob can be any size.</span></span> <span data-ttu-id="4d451-110">Blobbar kan vara till exempel bilder, ljud- och bildfiler, rådata och dokumentfiler.</span><span class="sxs-lookup"><span data-stu-id="4d451-110">Blobs can be things like images, audio and video files, raw data, and document files.</span></span>

<span data-ttu-id="4d451-111">Precis som filer live i mappar, live storage-blobbar i behållare.</span><span class="sxs-lookup"><span data-stu-id="4d451-111">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="4d451-112">När du har skapat en lagringsenhet, skapar du en eller flera behållare i hello lagring.</span><span class="sxs-lookup"><span data-stu-id="4d451-112">After you have created a storage, you create one or more containers in hello storage.</span></span> <span data-ttu-id="4d451-113">Till exempel i en lagring som kallas ”klippboken”, kan du skapa behållare i hello lagring som kallas ”bilder” toostore bilder och en annan som heter ”ljud” toostore ljudfiler.</span><span class="sxs-lookup"><span data-stu-id="4d451-113">For example, in a storage called "Scrapbook," you can create containers in hello storage called "images" toostore pictures and another called "audio" toostore audio files.</span></span> <span data-ttu-id="4d451-114">När du har skapat hello behållare kan du överföra enskilda blob filer toothem.</span><span class="sxs-lookup"><span data-stu-id="4d451-114">After you create hello containers, you can upload individual blob files toothem.</span></span>

* <span data-ttu-id="4d451-115">Mer information om programmässigt manipulering blobbar finns [komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="4d451-115">For more information on programmatically manipulating blobs, see [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md).</span></span>
* <span data-ttu-id="4d451-116">Allmän information om Azure Storage finns [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="4d451-116">For general information about Azure Storage, see [Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>
* <span data-ttu-id="4d451-117">Allmän information om Azure Cloud Services, se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/).</span><span class="sxs-lookup"><span data-stu-id="4d451-117">For general information about Azure Cloud Services, see [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/).</span></span>
* <span data-ttu-id="4d451-118">Mer information om programmering i ASP.NET-program finns [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="4d451-118">For more information about programming ASP.NET applications, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="4d451-119">Åtkomst till blob-behållare i koden</span><span class="sxs-lookup"><span data-stu-id="4d451-119">Access blob containers in code</span></span>
<span data-ttu-id="4d451-120">tooprogrammatically komma åt blobar i molntjänstprojekt måste tooadd hello följande objekt, om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="4d451-120">tooprogrammatically access blobs in cloud service projects, you need tooadd hello following items, if they're not already present.</span></span>

1. <span data-ttu-id="4d451-121">Lägg till följande kod namnområde deklarationer toohello upp i en C#-fil som du vill tooprogrammatically åtkomst till Azure Storage hello.</span><span class="sxs-lookup"><span data-stu-id="4d451-121">Add hello following code namespace declarations toohello top of any C# file in which you wish tooprogrammatically access Azure Storage.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="4d451-122">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="4d451-122">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="4d451-123">Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="4d451-123">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. <span data-ttu-id="4d451-124">Hämta en **CloudBlobClient** objekt tooreference en befintlig behållare på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="4d451-124">Get a **CloudBlobClient** object tooreference an existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. <span data-ttu-id="4d451-125">Hämta en **CloudBlobContainer** objekt tooreference en specifik blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="4d451-125">Get a **CloudBlobContainer** object tooreference a specific blob container.</span></span>
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> <span data-ttu-id="4d451-126">Använda alla hello koden som visas i hello ovan framför hello koden som visas i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="4d451-126">Use all of hello code shown in hello previous procedure in front of hello code shown in hello following sections.</span></span>
> 
> 

## <a name="create-a-container-in-code"></a><span data-ttu-id="4d451-127">Skapa en behållare i koden</span><span class="sxs-lookup"><span data-stu-id="4d451-127">Create a container in code</span></span>
> [!NOTE]
> <span data-ttu-id="4d451-128">Vissa API: er som utför anrop ut tooAzure lagring i ASP.NET är asynkron.</span><span class="sxs-lookup"><span data-stu-id="4d451-128">Some APIs that perform calls out tooAzure Storage in ASP.NET are asynchronous.</span></span> <span data-ttu-id="4d451-129">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4d451-129">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="4d451-130">hello koden i följande exempel hello förutsätter att du använder asynkrona programming metoder.</span><span class="sxs-lookup"><span data-stu-id="4d451-130">hello code in hello following example assumes that you are using async programming methods.</span></span>
> 
> 

<span data-ttu-id="4d451-131">toocreate en behållare i ditt lagringskonto, behöver du toodo är lägger till ett anrop för**CreateIfNotExistsAsync** som hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="4d451-131">toocreate a container in your storage account, all you need toodo is add a call too**CreateIfNotExistsAsync** as in hello following code:</span></span>

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="4d451-132">toomake hello filer i hello behållaren tillgängliga tooeveryone, du kan ange hello behållaren toobe offentlig med hjälp av hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="4d451-132">toomake hello files within hello container available tooeveryone, you can set hello container toobe public by using hello following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


<span data-ttu-id="4d451-133">Vem som helst på hello Internet kan se blobbar i en offentlig behållare, men du kan ändra eller ta bort dem bara om du har rätt åtkomstnyckel för hello.</span><span class="sxs-lookup"><span data-stu-id="4d451-133">Anyone on hello Internet can see blobs in a public container, but you can modify or delete them only if you have hello appropriate access key.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="4d451-134">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="4d451-134">Upload a blob into a container</span></span>
<span data-ttu-id="4d451-135">Azure Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="4d451-135">Azure Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="4d451-136">I hello flertalet fall är blockblob hello rekommenderade typen toouse.</span><span class="sxs-lookup"><span data-stu-id="4d451-136">In hello majority of cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="4d451-137">tooupload en fil tooa blockblob, hämta en referens för behållaren och använder det tooget en blockblobben.</span><span class="sxs-lookup"><span data-stu-id="4d451-137">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="4d451-138">När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **UploadFromStream** metod.</span><span class="sxs-lookup"><span data-stu-id="4d451-138">Once you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStream** method.</span></span> <span data-ttu-id="4d451-139">Den här åtgärden skapar hello blob om det inte fanns tidigare, eller skrivs över om den finns.</span><span class="sxs-lookup"><span data-stu-id="4d451-139">This operation creates hello blob if it didn't previously exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="4d451-140">följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="4d451-140">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

    // Retrieve a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="4d451-141">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="4d451-141">List hello blobs in a container</span></span>
<span data-ttu-id="4d451-142">toolist hello blobbar i en behållare först hämta en referens för behållaren.</span><span class="sxs-lookup"><span data-stu-id="4d451-142">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="4d451-143">Du kan sedan använda hello behållaren **ListBlobs** metoden tooretrieve hello blobbarna och/eller katalogerna i den.</span><span class="sxs-lookup"><span data-stu-id="4d451-143">You can then use hello container's **ListBlobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="4d451-144">tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **IListBlobItem**, måste du skicka den tooa **CloudBlockBlob**, **CloudPageBlob**, eller  **CloudBlobDirectory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4d451-144">tooaccess hello rich set of properties and methods for a  returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="4d451-145">Om hello typen är okänd, kan du använda en typ av kontroll toodetermine vilka toocast den.</span><span class="sxs-lookup"><span data-stu-id="4d451-145">If hello type is unknown, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="4d451-146">hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i hello **foton** behållare:</span><span class="sxs-lookup"><span data-stu-id="4d451-146">hello following code demonstrates how tooretrieve and output hello URI of each item in hello **photos** container:</span></span>

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

<span data-ttu-id="4d451-147">Enligt i hello kodexemplet har hello blob-tjänsten hello begreppet kataloger i behållare också.</span><span class="sxs-lookup"><span data-stu-id="4d451-147">As shown in hello previous code sample, hello blob service has hello concept of directories within containers, as well.</span></span> <span data-ttu-id="4d451-148">Detta är så att du kan ordna dina blobbar i en mer mapp-liknande struktur.</span><span class="sxs-lookup"><span data-stu-id="4d451-148">This is so that you can organize your blobs in a more folder-like structure.</span></span> <span data-ttu-id="4d451-149">Tänk dig följande uppsättning blockblobbar i en behållare med namnet hello **foton**:</span><span class="sxs-lookup"><span data-stu-id="4d451-149">For example, consider hello following set of block blobs in a container named **photos**:</span></span>

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

<span data-ttu-id="4d451-150">När du anropar **ListBlobs** på hello behållare (som hello föregående exempel), hello samlingen returnerade innehåller **CloudBlobDirectory** och **CloudBlockBlob** objekt som representerar hello kataloger respektive blobbar som finns på hello översta nivån.</span><span class="sxs-lookup"><span data-stu-id="4d451-150">When you call **ListBlobs** on hello container (as in hello previous sample), hello collection returned contains **CloudBlobDirectory** and **CloudBlockBlob** objects representing hello directories and blobs contained at hello top level.</span></span> <span data-ttu-id="4d451-151">Här är hello resulterande utdata:</span><span class="sxs-lookup"><span data-stu-id="4d451-151">Here is hello resulting output:</span></span>

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


<span data-ttu-id="4d451-152">Du kan ange hello **UseFlatBlobListing** för hello **ListBlobs** metod för att **SANT**.</span><span class="sxs-lookup"><span data-stu-id="4d451-152">Optionally, you can set hello **UseFlatBlobListing** parameter of of hello **ListBlobs** method to **true**.</span></span> <span data-ttu-id="4d451-153">Detta resulterar i varje blob som returneras som en **CloudBlockBlob**, oavsett directory.</span><span class="sxs-lookup"><span data-stu-id="4d451-153">This results in every blob being returned as a **CloudBlockBlob**, regardless of directory.</span></span> <span data-ttu-id="4d451-154">Här är hello anrop för**ListBlobs**:</span><span class="sxs-lookup"><span data-stu-id="4d451-154">Here is hello call too**ListBlobs**:</span></span>

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

<span data-ttu-id="4d451-155">och här finns hello resultat:</span><span class="sxs-lookup"><span data-stu-id="4d451-155">and here are hello results:</span></span>

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

<span data-ttu-id="4d451-156">Mer information finns i [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d451-156">For more information, see [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span></span>

## <a name="download-blobs"></a><span data-ttu-id="4d451-157">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="4d451-157">Download blobs</span></span>
<span data-ttu-id="4d451-158">toodownload blobbar först hämta en blobbreferens och sedan anropa hello **DownloadToStream** metod.</span><span class="sxs-lookup"><span data-stu-id="4d451-158">toodownload blobs, first retrieve a blob reference and then call hello **DownloadToStream** method.</span></span> <span data-ttu-id="4d451-159">hello följande exempel används hello **DownloadToStream** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="4d451-159">hello following example uses hello **DownloadToStream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents tooa file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

<span data-ttu-id="4d451-160">Du kan också använda hello **DownloadToStream** metoden toodownload hello innehållet i en blob som en textsträng.</span><span class="sxs-lookup"><span data-stu-id="4d451-160">You can also use hello **DownloadToStream** method toodownload hello contents of a blob as a text string.</span></span>

    // Get a reference tooa blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a><span data-ttu-id="4d451-161">Ta bort blobbar</span><span class="sxs-lookup"><span data-stu-id="4d451-161">Delete blobs</span></span>
<span data-ttu-id="4d451-162">toodelete blob först hämta en blobbreferens och anropar sedan den **ta bort** metod.</span><span class="sxs-lookup"><span data-stu-id="4d451-162">toodelete a blob, first get a blob reference and then call the **Delete** method.</span></span>

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="4d451-163">Visa en lista över blobbar på sidor asynkront</span><span class="sxs-lookup"><span data-stu-id="4d451-163">List blobs in pages asynchronously</span></span>
<span data-ttu-id="4d451-164">Om du visar ett stort antal blobbar eller om du vill toocontrol hello antalet resultat som returneras i samma åtgärd, kan du visa blobbar på resultatsidor.</span><span class="sxs-lookup"><span data-stu-id="4d451-164">If you are listing a large number of blobs, or you want toocontrol hello number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="4d451-165">Det här exemplet visar hur tooreturn resultat på sidor asynkront, så att körningen inte blockeras medan du väntar på tooreturn ett stort antal resultat.</span><span class="sxs-lookup"><span data-stu-id="4d451-165">This example shows how tooreturn results in pages asynchronously, so that execution is not blocked while waiting tooreturn a large set of results.</span></span>

<span data-ttu-id="4d451-166">Det här exemplet illustrerar en platt blob lista, men du kan också utföra en hierarkiskt ordnad lista genom att ange hello **useFlatBlobListing** parametern för hello **ListBlobsSegmentedAsync** metod för **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="4d451-166">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting hello **useFlatBlobListing** parameter of hello **ListBlobsSegmentedAsync** method too**false**.</span></span>

<span data-ttu-id="4d451-167">Eftersom hello exempelmetoden anropar en asynkron metod, den måste föregås hello **asynkrona** nyckelord och det måste returnera en **aktivitet** objekt.</span><span class="sxs-lookup"><span data-stu-id="4d451-167">Because hello sample method calls an asynchronous method, it must be prefaced with hello **async** keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="4d451-168">Hej await nyckelord som angetts för hello **ListBlobsSegmentedAsync** metoden pausar körningen av hello exempelmetoden tills listuppgiften hello har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4d451-168">hello await keyword specified for hello **ListBlobsSegmentedAsync** method suspends execution of hello sample method until hello listing task completes.</span></span>

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs toohello console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
        // When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
        do
        {
            // This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a><span data-ttu-id="4d451-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d451-169">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

