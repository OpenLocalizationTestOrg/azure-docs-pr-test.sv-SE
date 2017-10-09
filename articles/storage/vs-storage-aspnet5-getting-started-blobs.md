---
title: "aaaGet igång med blob storage och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur tooget igång med Azure Blob storage i ett projekt i Visual Studio ASP.NET Core när du har skapat ett lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: a4c31c08c38d99eb9c66fe2af72ca42fa3804688
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Kom igång med Azure Blob storage och Visual Studio anslutna tjänster (ASP.NET kärnor)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur tooget igång med Azure Blob storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ett projekt för ASP.NET Core hello Visual Studio Lägg till anslutna tjänster dialogrutan.

Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data som kan nås från var som helst i hello world via HTTP eller HTTPS. En enda blob kan vara valfri storlek. Blobbar kan vara till exempel bilder, ljud- och bildfiler, rådata och dokumentfiler. Den här artikeln beskriver hur tooget igång med blob storage när du har skapat ett Azure storage-konto med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan i ASP.NET Core-projekt.

Precis som filer live i mappar, live storage-blobbar i behållare. När du har skapat en lagringsenhet, skapar du en eller flera behållare i hello lagring. Till exempel i en lagring som kallas ”klippboken”, kan du skapa behållare i hello lagring som kallas ”bilder” toostore bilder och en annan som heter ”ljud” toostore ljudfiler. När du har skapat hello behållare kan du överföra enskilda blob filer toothem. Se [komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) mer information om programmässigt manipulering blobbar.

## <a name="access-blob-containers-in-code"></a>Åtkomst till blob-behållare i koden
tooprogrammatically komma åt blobar i ASP.NET Core projekt måste tooadd hello följande objekt, om de inte redan finns.

1. Lägg till hello följande kod namnområde deklarationer toohello upp i en C#-fil som du vill tooprogrammatically åtkomst till Azure storage.
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använda följande kod tooget hello anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **Obs:** använda alla hello ovan kod framför hello koden i hello följande avsnitt.
3. Använd en **CloudBlobClient** objekt tooget en **CloudBlobContainer** referens tooan befintlig behållare på ditt lagringskonto.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>Skapa en behållare i koden
Du kan också använda hello **CloudBlobClient** toocreate en behållare i ditt lagringskonto. Allt du behöver toodo är tooadd ett anrop för**CreateIfNotExistsAsync** som hello följande kod:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference tooa container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**Obs:** hello API: er som utför anrop tooAzure lagring i ASP.NET Core är asynkron. Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information. hello koden nedan förutsätter asynkrona programming metoder som används.

toomake hello filer i hello behållaren tillgängliga tooeveryone, du kan ange hello behållaren toobe offentlig med hjälp av hello följande kod.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
tooupload en blob-fil till en behållare och hämta en referens för behållaren och använder det tooget en blobbreferens. När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **UploadFromStreamAsync** metod. Den här åtgärden skapar hello blob om den inte redan har gjort det, eller skrivs över om den finns. följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.

    // Get a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with hello contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello blobbar i en behållare först hämta en referens för behållaren. Du kan sedan anropa hello behållaren **ListBlobsSegmentedAsync** metoden tooretrieve hello blobbarna och/eller katalogerna i den. tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **IListBlobItem**, måste du skicka den tooa **CloudBlockBlob**, **CloudPageBlob**, eller  **CloudBlobDirectory** objekt. Om du inte vet hello blob skriver kan du använda en typ av kontroll toodetermine vilka toocast den. hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i en behållare.

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

Det finns andra sätt toolist hello innehållet i en blob-behållare. Se [komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) för mer information.

## <a name="download-a-blob"></a>Ladda ned en blob
toodownload blob först hämta en referens toohello blob och sedan anropa hello **DownloadToStreamAsync** metod. hello följande exempel används hello **DownloadToStreamAsync** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara som en lokal fil.

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save hello blob contents tooa file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Det finns andra sätt toosave blobbar som filer. Se [komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) för mer information.

## <a name="delete-a-blob"></a>Ta bort en blob
toodelete blob först hämta en referens toohello blob och sedan anropa hello **DeleteAsync** metod på den.

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

