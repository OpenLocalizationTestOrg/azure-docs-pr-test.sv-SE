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
# <a name="get-started-with-azure-blob-storage-using-net"></a>Komma igång med Azure Blob Storage med hjälp av .NET

[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.

### <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar hur toowrite .NET kod för några vanliga scenarier som använder Azure Blob storage. I kursen beskrivs scenarier där du laddar upp, visar en lista över, laddar ned och tar bort blobbar.

**Krav:**

* [Microsoft Visual Studio](https://www.visualstudio.com/)
* [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* Ett [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Fler exempel
Ytterligare exempel med Blob Storage finns i [Komma igång med Azure Blob Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Du kan ladda ned exempelprogrammet hello och kör den, eller bläddra hello koden på GitHub.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Lägga till med hjälp av direktiv
Lägg till följande hello **med** direktiven toohello överkant hello `Program.cs` fil:

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-hello-connection-string"></a>Parsa anslutningssträngen för hello
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-blob-service-client"></a>Skapa hello Blob-klienten
Hej **CloudBlobClient** klassen kan du tooretrieve behållare och blobbar som lagras i Blob storage. Här är ett sätt toocreate hello-klienten:

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
Nu är du redo toowrite kod som läser data från och skriver tooBlob datalagring.

## <a name="create-a-container"></a>Skapa en behållare
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Det här exemplet illustrerar hur toocreate en behållare om den inte redan finns:

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

Som standard är hello nya behållaren privat, vilket innebär att du måste ange lagring få åtkomst till viktiga toodownload blobbar från den här behållaren. Om du vill toomake hello filer i hello behållaren tillgängliga tooeveryone, kan du ange hello behållaren toobe offentliga med hello följande kod:

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

Vem som helst på hello Internet kan se blobbar i en offentlig behållare. Du kan ändra eller ta bort dem bara om du har hello lämpligt konto åtkomst till nyckeln eller en signatur för delad åtkomst.

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
Azure Blob Storage stöder blockblobbar och sidblobbar.  I de flesta fall är blockblob hello rekommenderade typen toouse.

tooupload en fil tooa blockblob, hämta en referens för behållaren och använder det tooget en blockblobben. När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **UploadFromStream** metod. Den här åtgärden skapar hello blob om det inte fanns tidigare, eller skrivs över om den finns.

följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.

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

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello blobbar i en behållare först hämta en referens för behållaren. Du kan sedan använda hello behållaren **ListBlobs** metoden tooretrieve hello blobbarna och/eller katalogerna i den. tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **IListBlobItem**, måste du skicka den tooa **CloudBlockBlob**, **CloudPageBlob**, eller  **CloudBlobDirectory** objekt. Om hello typen är okänd, kan du använda en typ av kontroll toodetermine vilka toocast den. hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i hello _foton_ behållare:

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

Genom att ta med information om sökvägen i blobbnamnen kan du skapa en virtuell katalogstruktur som du kan ordna och bläddra i på samma sätt som ett traditionellt filsystem. hello katalogstrukturen är virtuell endast--hello endast tillgängliga resurserna i Blob storage är behållare och blobbar. Dock hello klientbiblioteket tillhandahåller en **CloudBlobDirectory** objekt toorefer tooa virtuell katalog och förenkla processen hello arbetet med blobbar som är ordnade på det här sättet.

Tänk dig följande uppsättning blockblobbar i en behållare med namnet hello *foton*:

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

När du anropar **ListBlobs** på hello *foton* behållare (som hello som föregående kodfragment IT-avdelning), returneras en hierarkiskt ordnad lista. Den innehåller både **CloudBlobDirectory** och **CloudBlockBlob** objekt, som representerar hello kataloger respektive blobbar i hello-behållaren. hello resultatet ser ut så:

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

Du kan ange hello **UseFlatBlobListing** parametern för hello **ListBlobs** metod för att **SANT**. I detta fall returneras varje blobb i behållaren hello som en **CloudBlockBlob** objekt. Hej anrop för**ListBlobs** tooreturn en platt lista ser ut så här:

```csharp
// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

och hello resultatet ser ut så här:

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

## <a name="download-blobs"></a>Ladda ned blobbar
toodownload blobbar först hämta en blobbreferens och sedan anropa hello **DownloadToStream** metod. hello följande exempel används hello **DownloadToStream** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara tooa lokal fil.

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

Du kan också använda hello **DownloadToStream** metoden toodownload hello innehållet i en blob som en textsträng.

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

## <a name="delete-blobs"></a>Ta bort blobbar
toodelete blob först hämta en blobbreferens och anropar sedan den **ta bort** metod på den.

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

## <a name="list-blobs-in-pages-asynchronously"></a>Visa en lista över blobbar på sidor asynkront
Om du visar ett stort antal blobbar eller om du vill toocontrol hello antalet resultat som returneras i samma åtgärd, kan du visa blobbar på resultatsidor. Det här exemplet visar hur tooreturn resultat på sidor asynkront, så att körningen inte blockeras medan du väntar på tooreturn ett stort antal resultat.

Det här exemplet illustrerar en platt blob lista, men du kan också utföra en hierarkiskt ordnad lista genom att ange hello _useFlatBlobListing_ parametern för hello **ListBlobsSegmentedAsync** metoden too_false_.

Eftersom hello exempelmetoden anropar en asynkron metod, den måste föregås hello _asynkrona_ nyckelord och det måste returnera en **aktivitet** objekt. Hej await nyckelord som angetts för hello **ListBlobsSegmentedAsync** metoden pausar körningen av hello exempelmetoden tills listuppgiften hello har slutförts.

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

## <a name="writing-tooan-append-blob"></a>Skrivning tooan lägga till blob
En tilläggsblobb är optimerad för tilläggsåtgärder, t.ex loggning. Som en blockblobb består en tilläggsblobb av block, men när du lägger till en ny tilläggsblobb för block tooan det är alltid tillagda toohello slutet av hello-blob. Du kan inte uppdatera eller ta bort ett befintligt block i en tilläggsblobb. hello block ID för en tilläggsblobb exponeras inte eftersom de är för en blockblob.

Varje block i en tilläggsblobb kan ha olika storlek, upp tooa högst 4 MB och en tilläggsblobb kan innehålla högst 50 000 block. hello maximal storlek på en tilläggsblobb är därför lite över 195 GB (4 MB × 50 000 block).

hello exemplet nedan skapar en ny tilläggsblobb och lägger till vissa data tooit, simulera en enkel loggningsåtgärd.

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

Se [förstå Blockblobbar, Sidblobbar och Tilläggsblobbar](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) för mer information om hello skillnaderna mellan hello tre typer av blobbar.

## <a name="managing-security-for-blobs"></a>Hantera säkerheten för blobbar
Standard skydda Azure Storage dina data genom att begränsa åtkomst toohello kontoägaren, som innehar hello åtkomstnycklarna för kontot. När du behöver tooshare blob-data i ditt lagringskonto är det viktigt toodo så utan att kompromissa med hello säkerheten för åtkomstnycklarna för ditt konto. Dessutom kan du kryptera blob data tooensure som de är skyddade över hello överföring och i Azure Storage.

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-tooblob-data"></a>Hur du styr åtkomst till tooblob data
Hello blob-data i ditt lagringskonto är tillgänglig endast toostorage kontoägaren som standard. Autentisering av förfrågningar mot Blob storage krävs åtkomstnyckeln hello som standard. Dock gärna toomake vissa blob-data tillgängliga tooother användare. Du kan välja mellan två alternativ:

* **Anonym åtkomst:** Du kan göra en behållare eller dess blobbar offentligt tillgängliga för anonym åtkomst. Se [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md) för mer information.
* **Signaturer för delad åtkomst:** du kan ge klienterna en signatur för delad åtkomst (SAS), som ger delegerad åtkomst tooa resurs i ditt lagringskonto med behörigheter som du anger och under den period som du anger. Mer information finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md).

### <a name="encrypting-blob-data"></a>Kryptera blobbdata
Azure Storage har stöd för kryptering av blobbdata både på hello klient och server hello:

* **Kryptering på klientsidan:** hello Storage-klientbibliotek för .NET har stöd för kryptering av data i klientprogram före överföringen tooAzure lagring och dekryptera data vid hämtning av toohello klienten. hello biblioteket stöder även integrering med Azure Key Vault storage-konto för hantering av nycklar. Mer information finns i [Kryptering på klientsidan med .NET för Microsoft Azure Storage](storage-client-side-encryption.md). Se även [Självstudier: Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).
* **Kryptering på serversidan**: Nu stöder Azure Storage kryptering på serversidan. Mer information finns i [Azure Storage Service-kryptering av vilande data (förhandsgranskning)](storage-service-encryption.md).

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Blob storage följa dessa länkar toolearn mer.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.

### <a name="blob-storage-samples"></a>Exempel på Blob Storage (förhandsversion)
* [Komma igång med Azure Blob Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Referens för Blob Storage
* [Storage-klientbibliotek för .NET-referens](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [REST API-referens](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a>Begreppsorienterade guider
* [Överföra data med hello kommandoradsverktyget azcopy](storage-use-azcopy.md)
* [Komma igång med File Storage för .NET](storage-dotnet-how-to-use-files.md)
* [Hur toouse Azure blob storage med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
