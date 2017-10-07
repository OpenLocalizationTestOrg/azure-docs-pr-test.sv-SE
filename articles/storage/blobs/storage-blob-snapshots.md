---
title: "aaaCreate en skrivskyddad ögonblicksbild av en blobb i Azure Storage | Microsoft Docs"
description: "Lär dig hur toocreate en ögonblicksbild av en blob-tooback in blob-data vid en given tidpunkt. Förstå hur ögonblicksbilder debiteras och toouse dem toominimize kapacitet avgifter."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 3710705d-e127-4b01-8d0f-29853fb06d0d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: marsma
ms.openlocfilehash: 57f2e76b8899b8a513688bf148dd13673141d5bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-blob-snapshot"></a>Skapa en blob-ögonblicksbild

En ögonblicksbild är en skrivskyddad version av en blob som utförs i en punkt i tiden. Ögonblicksbilder är användbara för att säkerhetskopiera blobbar. När du har skapat en ögonblicksbild, läsa, kopiera eller ta bort den, men du kan inte ändra den.

En ögonblicksbild av en blob är identiska tooits grundläggande blob, förutom att hello blob-URI: N har en **DateTime** värde läggs toohello blob URI tooindicate hello tid vid vilken hello ögonblicksbilden togs. Till exempel om en sida blob-URI är `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello ögonblicksbild URI är liknande för`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Alla ögonblicksbilder dela hello grundläggande blob-URI. Hej endast är skillnaden mellan grundläggande hello-blob och hello ögonblicksbild hello läggs **DateTime** värde.
>

En blob kan ha valfritt antal ögonblicksbilder. Ögonblicksbilder sparas tills de uttryckligen tas bort. En ögonblicksbild inte sträcker sig längre än dess grundläggande blob. Du kan räkna upp hello ögonblicksbilder kopplade till hello grundläggande blob tootrack din aktuella ögonblicksbilder.

När du skapar en ögonblicksbild av en blob hello blob Systemegenskaper är kopierade toohello ögonblicksbild med hello samma värden. hello grundläggande blob-metadata är också kopierade toohello ögonblicksbild, såvida du inte anger separat metadata för hello när du skapar den en ögonblicksbild.

Alla lån som är associerade med grundläggande hello-blob påverkar inte hello ögonblicksbild. Du kan inte hämta ett lån på en ögonblicksbild.

En VHD-fil är används toostore hello aktuell information och status för en virtuell disk. Du kan koppla bort en disk från inom hello VM eller Stäng av hello VM och sedan ta en ögonblicksbild av dess VHD-filen. Du kan använda filen ögonblicksbild senare tooretrieve hello VHD-filen då i tid och återskapa hello VM.

Om kryptering för lagring-tjänsten (SSE) är aktiverat för hello storage-konto i vilka hello blob finns och sedan krypteras alla ögonblicksbilder av blobben i vila.

## <a name="create-a-snapshot"></a>Skapa en ögonblicksbild
hello följande kodexempel visar hur toocreate en ögonblicksbild med hjälp av hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Det här exemplet anger ytterligare metadata för hello ögonblicksbilden när den skapas.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in hello container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload hello blob toocreate it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of hello base blob.
        // Specify metadata at hello time that hello snapshot is created toospecify unique metadata for hello snapshot.
        // If no metadata is specified when hello snapshot is created, hello base blob's metadata is copied toohello snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="copy-snapshots"></a>Kopiera ögonblicksbilder
Kopieringsåtgärder som rör blobbar och ögonblicksbilder följer dessa regler:

* Du kan kopiera en ögonblicksbild över dess grundläggande blob. Du kan återställa en tidigare version av en blob genom att befordra en ögonblicksbild toohello position grundläggande hello-blob. Hej ögonblicksbild fortfarande, men hello grundläggande blob skrivs över med en skrivbar kopia av hello ögonblicksbild.
* Du kan kopiera en ögonblicksbild tooa mål-blob med ett annat namn. hello resulterande mål-blob är en skrivbar blob och inte en ögonblicksbild.
* När en käll-blob har kopierats så är inte alla ögonblicksbilder av hello källa blob kopierade toohello mål. När en mål-blob skrivs över med en kopia, bevaras eventuella ögonblicksbilder som är associerade med hello ursprungliga mål-blob.
* När du skapar en ögonblicksbild av en blockblobb är också hello blob allokerat Blockeringslista kopierade toohello ögonblicksbild. Alla ogenomförda block kopieras inte.

## <a name="specify-an-access-condition"></a>Ange ett villkor för åtkomst
När du anropar [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], du kan ange ett villkor för åtkomst så att hello ögonblicksbild skapas endast om ett villkor uppfylls. toospecify ett åtkomst-villkor använda hello [AccessCondition] [ dotnet_AccessCondition] parameter. Om hello anges inte är uppfyllt, hello ögonblicksbild har inte skapats och hello Blob-tjänsten returnerar statuskod [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.

## <a name="delete-snapshots"></a>Ta bort ögonblicksbilder
Du kan inte ta bort en blob med ögonblicksbilder om hello-ögonblicksbilder tas också bort. Du kan ta bort en ögonblicksbild individuellt eller ange att alla ögonblicksbilder tas bort när hello källa blob tas bort. Om du försöker toodelete en blob som fortfarande har ögonblicksbilder, uppstår ett fel.

Hej följande exempel visas hur toodelete en blob och dess ögonblicksbilder i .NET, där `blockBlob` är ett objekt av typen [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Ögonblicksbilder med Azure Premium-lagring
När du använder ögonblicksbilder med Premium-lagring, tillämpa hello följande regler:

* hello maximalt antal ögonblicksbilder per sidblob i ett premiumlagringskonto är 100. Om denna gräns har överskridits hello ögonblicksbild Blob-åtgärd returnerar en felkod 409 (`SnapshotCountExceeded`).
* Du kan ta en ögonblicksbild av en sidblob i ett premiumlagringskonto var 10: e minut. Om denna kurs har överskridits hello ögonblicksbild Blob-åtgärd returnerar en felkod 409 (`SnapshotOperationRateExceeded`).
* tooread en ögonblicksbild, du kan använda hello kopiera Blob åtgärden toocopy en ögonblicksbild tooanother sidblob i hello-konto. hello mål-blob för hello kopieringsåtgärden får inte ha några befintliga ögonblicksbilder. Om hello mål-blob har ögonblicksbilder, hello kopiera Blob-åtgärd och returnerar sedan felkoden 409 (`SnapshotsPresent`).

## <a name="return-hello-absolute-uri-tooa-snapshot"></a>Returnera hello absolut URI tooa ögonblicksbild
Den här C#-kodexempel skapar en ögonblicksbild och skriver ut hello absolut URI för hello primär plats.

```csharp
//Create hello blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference tooa blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of hello blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Förstå hur ögonblicksbilder påförs kostnader
Skapa en ögonblicksbild som är en skrivskyddad kopia av en blob, kan leda till ytterligare data lagringskonto avgifter tooyour. När du skapar ditt program, är det viktigt toobe som är medvetna om hur dessa avgifter kan tillkomma så att du kan minimera kostnaderna.

### <a name="important-billing-considerations"></a>Tänk på följande fakturering
hello följande lista innehåller nyckeln pekar tooconsider när du skapar en ögonblicksbild.

* Ditt lagringskonto debiteras för unik block eller sidor, oavsett om de är i hello blob eller hello ögonblicksbild. Ditt konto leder inte till ytterligare avgifter för ögonblicksbilder kopplade till en blob förrän du har uppdaterat hello blob som de är baserad på. När du har uppdaterat hello grundläggande blob skiljer sig från dess ögonblicksbilder. När detta inträffar debiteras du för hello unika block eller sidor i varje blob eller en ögonblicksbild.
* När du ersätter ett block inom en blockblobb debiteras som blockerar senare som en unik block. Detta gäller även om hello block har hello samma blockera ID och hello samma data som den har i hello ögonblicksbild. När hello block strävar igen och det avviker från sin motsvarighet i en ögonblicksbild och du kommer att debiteras för sina data. hello detsamma gäller för en sida i en sidblob som uppdateras med identiska data.
* Ersätta en blockblobb genom att anropa hello [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], eller [UploadFromByteArray] [ dotnet_UploadFromByteArray] metoden ersätter alla block i hello-blob. Om du har en ögonblicksbild som är associerade med blobben alla block i grundläggande hello-blob och ögonblicksbild nu avvika och du kommer att debiteras för alla hello block i båda blobbar. Detta gäller även om hello data i grundläggande hello-blob och hello ögonblicksbild vara identiska.
* hello Azure Blob-tjänsten har inte en innebär toodetermine om två block innehåller samma data. Varje block som har överförts och verkställs behandlas som unika, även om den har hello samma data och hello samma blockera ID. Eftersom det tillkomma avgifter för unik block, är det viktigt tooconsider som uppdaterar en blob som har en ögonblicksbild resulterar i ytterligare unika block och ytterligare kostnader.

### <a name="minimize-cost-with-snapshot-management"></a>Minimera kostnad med hantering av ögonblicksbilder

Vi rekommenderar att du hanterar din ögonblicksbilder noggrant tooavoid extra kostnader. Du kan följa dessa rekommendationer toohelp minimera hello kostnader hello lagring av ögonblicksbilder av din:

* Ta bort och återskapa ögonblicksbilder kopplade till en blob när du uppdaterar hello blob, även om du uppdaterar med identiska data om din program-designen kräver att du behåller ögonblicksbilder. Genom att ta bort och återskapa hello blob ögonblicksbilder, kan du se till att hello blob och ögonblicksbilder inte sig.
* Om du underhåller ögonblicksbilder för en blob och undvika anropar [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], eller [UploadFromByteArray] [ dotnet_UploadFromByteArray] tooupdate hello blob. Dessa metoder ersätta alla hello block i hello blob, orsakar din grundläggande blob och dess ögonblicksbilder toodiverge avsevärt. I stället uppdatera hello lägsta möjliga antal block med hjälp av hello [PutBlock] [ dotnet_PutBlock] och [PutBlockList] [ dotnet_PutBlockList] metoder.

### <a name="snapshot-billing-scenarios"></a>Ögonblicksbild fakturering scenarier
hello följande scenarier visas hur debiteringar görs för en blockblobb och dess ögonblicksbilder.

**Scenario 1**

I scenario 1 har grundläggande hello-blob inte uppdaterats efter hello ögonblicksbilden togs, så avgifter tas ut endast unika block 1, 2 och 3.

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**Scenario 2**

I scenario 2 grundläggande hello-blob har uppdaterats, men hello ögonblicksbild har inte. Blockera 3 har uppdaterats och även om den innehåller hello samma data och hello samma ID kan inte hello samma som blockerar 3 i hello ögonblicksbild. Därför debiteras hello kontot för fyra block.

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**Scenario 3**

I scenario 3 grundläggande hello-blob har uppdaterats, men hello ögonblicksbild har inte. Block 3 ersattes med block 4 i grundläggande hello-blob, men visar hello ögonblicksbild block 3. Därför debiteras hello kontot för fyra block.

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**Scenario 4**

I scenariot 4 grundläggande hello-blob har uppdaterats helt och innehåller inga av dess ursprungliga block. Därför debiteras hello kontot för alla åtta unika block. Den här situationen kan uppstå om du använder en metod för att uppdatera som [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], eller [UploadFromByteArray][dotnet_UploadFromByteArray], eftersom dessa metoder ersätta alla hello innehållet i en blob.

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Nästa steg

* Du hittar mer information om hur du arbetar med ögonblicksbilder av virtuella datorer (VM)-disk i [säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder](../../virtual-machines/windows/incremental-snapshots.md)

* Ytterligare kodexempel med Blob storage finns [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Du kan hämta ett exempelprogram och kör den, eller bläddra hello koden på GitHub.

[dotnet_AccessCondition]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.accesscondition.aspx
[dotnet_CloudBlockBlob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx
[dotnet_CreateSnapshotAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.createsnapshotasync.aspx
[dotnet_HTTPStatusCode]: https://msdn.microsoft.com/library/system.net.httpstatuscode(v=vs.110).aspx
[dotnet_PutBlockList]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblocklist.aspx
[dotnet_PutBlock]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblock.aspx
[dotnet_UploadFromByteArray]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfrombytearray.aspx
[dotnet_UploadFromFile]: https://msdn.microsoft.com/library/azure/mt705654.aspx
[dotnet_UploadFromStream]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstream.aspx
[dotnet_UploadText]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadtext.aspx