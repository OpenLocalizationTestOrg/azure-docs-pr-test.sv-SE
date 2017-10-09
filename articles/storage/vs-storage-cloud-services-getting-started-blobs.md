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
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Kom igång med Azure Blob Storage och Visual Studio anslutna tjänster (cloud services-projekt)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur tooget igång med Azure Blob Storage när du har skapat eller refererar till ett Azure Storage-konto med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan i ett Visual Studio-moln services-projekt. Får du lära dig hur tooaccess och skapa blob-behållare och hur tooperform vanliga uppgifter som överföra, lista och ladda ned blobbar. hello prover är skrivna i C\# och använda hello [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure Blob Storage är en tjänst för att lagra stora mängder Ostrukturerade data som kan nås från var som helst i hello world via HTTP eller HTTPS. En enda blob kan vara valfri storlek. Blobbar kan vara till exempel bilder, ljud- och bildfiler, rådata och dokumentfiler.

Precis som filer live i mappar, live storage-blobbar i behållare. När du har skapat en lagringsenhet, skapar du en eller flera behållare i hello lagring. Till exempel i en lagring som kallas ”klippboken”, kan du skapa behållare i hello lagring som kallas ”bilder” toostore bilder och en annan som heter ”ljud” toostore ljudfiler. När du har skapat hello behållare kan du överföra enskilda blob filer toothem.

* Mer information om programmässigt manipulering blobbar finns [komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md).
* Allmän information om Azure Storage finns [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/).
* Allmän information om Azure Cloud Services, se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/).
* Mer information om programmering i ASP.NET-program finns [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Åtkomst till blob-behållare i koden
tooprogrammatically komma åt blobar i molntjänstprojekt måste tooadd hello följande objekt, om de inte redan finns.

1. Lägg till följande kod namnområde deklarationer toohello upp i en C#-fil som du vill tooprogrammatically åtkomst till Azure Storage hello.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. Hämta en **CloudBlobClient** objekt tooreference en befintlig behållare på ditt lagringskonto.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. Hämta en **CloudBlobContainer** objekt tooreference en specifik blob-behållare.
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> Använda alla hello koden som visas i hello ovan framför hello koden som visas i följande avsnitt hello.
> 
> 

## <a name="create-a-container-in-code"></a>Skapa en behållare i koden
> [!NOTE]
> Vissa API: er som utför anrop ut tooAzure lagring i ASP.NET är asynkron. Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information. hello koden i följande exempel hello förutsätter att du använder asynkrona programming metoder.
> 
> 

toocreate en behållare i ditt lagringskonto, behöver du toodo är lägger till ett anrop för**CreateIfNotExistsAsync** som hello följande kod:

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


toomake hello filer i hello behållaren tillgängliga tooeveryone, du kan ange hello behållaren toobe offentlig med hjälp av hello följande kod.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Vem som helst på hello Internet kan se blobbar i en offentlig behållare, men du kan ändra eller ta bort dem bara om du har rätt åtkomstnyckel för hello.

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
Azure Storage stöder blockblobbar och sidblobbar. I hello flertalet fall är blockblob hello rekommenderade typen toouse.

tooupload en fil tooa blockblob, hämta en referens för behållaren och använder det tooget en blockblobben. När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **UploadFromStream** metod. Den här åtgärden skapar hello blob om det inte fanns tidigare, eller skrivs över om den finns. följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.

    // Retrieve a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello blobbar i en behållare först hämta en referens för behållaren. Du kan sedan använda hello behållaren **ListBlobs** metoden tooretrieve hello blobbarna och/eller katalogerna i den. tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **IListBlobItem**, måste du skicka den tooa **CloudBlockBlob**, **CloudPageBlob**, eller  **CloudBlobDirectory** objekt. Om hello typen är okänd, kan du använda en typ av kontroll toodetermine vilka toocast den. hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i hello **foton** behållare:

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

Enligt i hello kodexemplet har hello blob-tjänsten hello begreppet kataloger i behållare också. Detta är så att du kan ordna dina blobbar i en mer mapp-liknande struktur. Tänk dig följande uppsättning blockblobbar i en behållare med namnet hello **foton**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

När du anropar **ListBlobs** på hello behållare (som hello föregående exempel), hello samlingen returnerade innehåller **CloudBlobDirectory** och **CloudBlockBlob** objekt som representerar hello kataloger respektive blobbar som finns på hello översta nivån. Här är hello resulterande utdata:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Du kan ange hello **UseFlatBlobListing** för hello **ListBlobs** metod för att **SANT**. Detta resulterar i varje blob som returneras som en **CloudBlockBlob**, oavsett directory. Här är hello anrop för**ListBlobs**:

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

och här finns hello resultat:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Mer information finns i [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Ladda ned blobbar
toodownload blobbar först hämta en blobbreferens och sedan anropa hello **DownloadToStream** metod. hello följande exempel används hello **DownloadToStream** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara tooa lokal fil.

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents tooa file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Du kan också använda hello **DownloadToStream** metoden toodownload hello innehållet i en blob som en textsträng.

    // Get a reference tooa blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Ta bort blobbar
toodelete blob först hämta en blobbreferens och anropar sedan den **ta bort** metod.

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Visa en lista över blobbar på sidor asynkront
Om du visar ett stort antal blobbar eller om du vill toocontrol hello antalet resultat som returneras i samma åtgärd, kan du visa blobbar på resultatsidor. Det här exemplet visar hur tooreturn resultat på sidor asynkront, så att körningen inte blockeras medan du väntar på tooreturn ett stort antal resultat.

Det här exemplet illustrerar en platt blob lista, men du kan också utföra en hierarkiskt ordnad lista genom att ange hello **useFlatBlobListing** parametern för hello **ListBlobsSegmentedAsync** metod för **FALSKT**.

Eftersom hello exempelmetoden anropar en asynkron metod, den måste föregås hello **asynkrona** nyckelord och det måste returnera en **aktivitet** objekt. Hej await nyckelord som angetts för hello **ListBlobsSegmentedAsync** metoden pausar körningen av hello exempelmetoden tills listuppgiften hello har slutförts.

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

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

