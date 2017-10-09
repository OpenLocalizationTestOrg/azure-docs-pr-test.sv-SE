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
ms.openlocfilehash: 9e7af611e885d83df69d3329217f261b3f3f333e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-blob-snapshot"></a><span data-ttu-id="5e409-104">Skapa en blob-ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="5e409-104">Create a blob snapshot</span></span>

<span data-ttu-id="5e409-105">En ögonblicksbild är en skrivskyddad version av en blob som utförs i en punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="5e409-105">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="5e409-106">Ögonblicksbilder är användbara för att säkerhetskopiera blobbar.</span><span class="sxs-lookup"><span data-stu-id="5e409-106">Snapshots are useful for backing up blobs.</span></span> <span data-ttu-id="5e409-107">När du har skapat en ögonblicksbild, läsa, kopiera eller ta bort den, men du kan inte ändra den.</span><span class="sxs-lookup"><span data-stu-id="5e409-107">After you create a snapshot, you can read, copy, or delete it, but you cannot modify it.</span></span>

<span data-ttu-id="5e409-108">En ögonblicksbild av en blob är identiska tooits grundläggande blob, förutom att hello blob-URI: N har en **DateTime** värde läggs toohello blob URI tooindicate hello tid vid vilken hello ögonblicksbilden togs.</span><span class="sxs-lookup"><span data-stu-id="5e409-108">A snapshot of a blob is identical tooits base blob, except that hello blob URI has a **DateTime** value appended toohello blob URI tooindicate hello time at which hello snapshot was taken.</span></span> <span data-ttu-id="5e409-109">Till exempel om en sida blob-URI är `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello ögonblicksbild URI är liknande för`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span><span class="sxs-lookup"><span data-stu-id="5e409-109">For example, if a page blob URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello snapshot URI is similar too`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span></span>

> [!NOTE]
> <span data-ttu-id="5e409-110">Alla ögonblicksbilder dela hello grundläggande blob-URI.</span><span class="sxs-lookup"><span data-stu-id="5e409-110">All snapshots share hello base blob's URI.</span></span> <span data-ttu-id="5e409-111">Hej endast är skillnaden mellan grundläggande hello-blob och hello ögonblicksbild hello läggs **DateTime** värde.</span><span class="sxs-lookup"><span data-stu-id="5e409-111">hello only distinction between hello base blob and hello snapshot is hello appended **DateTime** value.</span></span>
>

<span data-ttu-id="5e409-112">En blob kan ha valfritt antal ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="5e409-112">A blob can have any number of snapshots.</span></span> <span data-ttu-id="5e409-113">Ögonblicksbilder sparas tills de uttryckligen tas bort.</span><span class="sxs-lookup"><span data-stu-id="5e409-113">Snapshots persist until they are explicitly deleted.</span></span> <span data-ttu-id="5e409-114">En ögonblicksbild inte sträcker sig längre än dess grundläggande blob.</span><span class="sxs-lookup"><span data-stu-id="5e409-114">A snapshot cannot outlive its base blob.</span></span> <span data-ttu-id="5e409-115">Du kan räkna upp hello ögonblicksbilder kopplade till hello grundläggande blob tootrack din aktuella ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="5e409-115">You can enumerate hello snapshots associated with hello base blob tootrack your current snapshots.</span></span>

<span data-ttu-id="5e409-116">När du skapar en ögonblicksbild av en blob hello blob Systemegenskaper är kopierade toohello ögonblicksbild med hello samma värden.</span><span class="sxs-lookup"><span data-stu-id="5e409-116">When you create a snapshot of a blob, hello blob's system properties are copied toohello snapshot with hello same values.</span></span> <span data-ttu-id="5e409-117">hello grundläggande blob-metadata är också kopierade toohello ögonblicksbild, såvida du inte anger separat metadata för hello när du skapar den en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-117">hello base blob's metadata is also copied toohello snapshot, unless you specify separate metadata for hello snapshot when you create it.</span></span>

<span data-ttu-id="5e409-118">Alla lån som är associerade med grundläggande hello-blob påverkar inte hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-118">Any leases associated with hello base blob do not affect hello snapshot.</span></span> <span data-ttu-id="5e409-119">Du kan inte hämta ett lån på en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-119">You cannot acquire a lease on a snapshot.</span></span>

<span data-ttu-id="5e409-120">En VHD-fil är används toostore hello aktuell information och status för en virtuell disk.</span><span class="sxs-lookup"><span data-stu-id="5e409-120">A VHD file is used toostore hello current information and status for a VM disk.</span></span> <span data-ttu-id="5e409-121">Du kan koppla bort en disk från inom hello VM eller Stäng av hello VM och sedan ta en ögonblicksbild av dess VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="5e409-121">You can detach a disk from within hello VM or shut down hello VM, and then take a snapshot of its VHD file.</span></span> <span data-ttu-id="5e409-122">Du kan använda filen ögonblicksbild senare tooretrieve hello VHD-filen då i tid och återskapa hello VM.</span><span class="sxs-lookup"><span data-stu-id="5e409-122">You can use that snapshot file later tooretrieve hello VHD file at that point in time and recreate hello VM.</span></span>

<span data-ttu-id="5e409-123">Om kryptering för lagring-tjänsten (SSE) är aktiverat för hello storage-konto i vilka hello blob finns och sedan krypteras alla ögonblicksbilder av blobben i vila.</span><span class="sxs-lookup"><span data-stu-id="5e409-123">If Storage Service Encryption (SSE) is enabled for hello storage account in which hello blob resides, then any snapshots taken of that blob will be encrypted at rest.</span></span>

## <a name="create-a-snapshot"></a><span data-ttu-id="5e409-124">Skapa en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="5e409-124">Create a snapshot</span></span>
<span data-ttu-id="5e409-125">hello följande kodexempel visar hur toocreate en ögonblicksbild med hjälp av hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="5e409-125">hello following code example shows how toocreate a snapshot by using hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span> <span data-ttu-id="5e409-126">Det här exemplet anger ytterligare metadata för hello ögonblicksbilden när den skapas.</span><span class="sxs-lookup"><span data-stu-id="5e409-126">This example specifies additional metadata for hello snapshot when it is created.</span></span>

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

## <a name="copy-snapshots"></a><span data-ttu-id="5e409-127">Kopiera ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="5e409-127">Copy snapshots</span></span>
<span data-ttu-id="5e409-128">Kopieringsåtgärder som rör blobbar och ögonblicksbilder följer dessa regler:</span><span class="sxs-lookup"><span data-stu-id="5e409-128">Copy operations involving blobs and snapshots follow these rules:</span></span>

* <span data-ttu-id="5e409-129">Du kan kopiera en ögonblicksbild över dess grundläggande blob.</span><span class="sxs-lookup"><span data-stu-id="5e409-129">You can copy a snapshot over its base blob.</span></span> <span data-ttu-id="5e409-130">Du kan återställa en tidigare version av en blob genom att befordra en ögonblicksbild toohello position grundläggande hello-blob.</span><span class="sxs-lookup"><span data-stu-id="5e409-130">By promoting a snapshot toohello position of hello base blob, you can restore an earlier version of a blob.</span></span> <span data-ttu-id="5e409-131">Hej ögonblicksbild fortfarande, men hello grundläggande blob skrivs över med en skrivbar kopia av hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-131">hello snapshot remains, but hello base blob is overwritten with a writable copy of hello snapshot.</span></span>
* <span data-ttu-id="5e409-132">Du kan kopiera en ögonblicksbild tooa mål-blob med ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="5e409-132">You can copy a snapshot tooa destination blob with a different name.</span></span> <span data-ttu-id="5e409-133">hello resulterande mål-blob är en skrivbar blob och inte en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-133">hello resulting destination blob is a writable blob and not a snapshot.</span></span>
* <span data-ttu-id="5e409-134">När en käll-blob har kopierats så är inte alla ögonblicksbilder av hello källa blob kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="5e409-134">When a source blob is copied, any snapshots of hello source blob are not copied toohello destination.</span></span> <span data-ttu-id="5e409-135">När en mål-blob skrivs över med en kopia, bevaras eventuella ögonblicksbilder som är associerade med hello ursprungliga mål-blob.</span><span class="sxs-lookup"><span data-stu-id="5e409-135">When a destination blob is overwritten with a copy, any snapshots associated with hello original destination blob remain intact.</span></span>
* <span data-ttu-id="5e409-136">När du skapar en ögonblicksbild av en blockblobb är också hello blob allokerat Blockeringslista kopierade toohello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-136">When you create a snapshot of a block blob, hello blob's committed block list is also copied toohello snapshot.</span></span> <span data-ttu-id="5e409-137">Alla ogenomförda block kopieras inte.</span><span class="sxs-lookup"><span data-stu-id="5e409-137">Any uncommitted blocks are not copied.</span></span>

## <a name="specify-an-access-condition"></a><span data-ttu-id="5e409-138">Ange ett villkor för åtkomst</span><span class="sxs-lookup"><span data-stu-id="5e409-138">Specify an access condition</span></span>
<span data-ttu-id="5e409-139">När du anropar [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], du kan ange ett villkor för åtkomst så att hello ögonblicksbild skapas endast om ett villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="5e409-139">When you call [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], you can specify an access condition so that hello snapshot is created only if a condition is met.</span></span> <span data-ttu-id="5e409-140">toospecify ett åtkomst-villkor använda hello [AccessCondition] [ dotnet_AccessCondition] parameter.</span><span class="sxs-lookup"><span data-stu-id="5e409-140">toospecify an access condition, use hello [AccessCondition][dotnet_AccessCondition] parameter.</span></span> <span data-ttu-id="5e409-141">Om hello anges inte är uppfyllt, hello ögonblicksbild har inte skapats och hello Blob-tjänsten returnerar statuskod [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.</span><span class="sxs-lookup"><span data-stu-id="5e409-141">If hello specified condition is not met, hello snapshot is not created, and hello Blob service returns status code [HTTPStatusCode][dotnet_HTTPStatusCode].PreconditionFailed.</span></span>

## <a name="delete-snapshots"></a><span data-ttu-id="5e409-142">Ta bort ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="5e409-142">Delete snapshots</span></span>
<span data-ttu-id="5e409-143">Du kan inte ta bort en blob med ögonblicksbilder om hello-ögonblicksbilder tas också bort.</span><span class="sxs-lookup"><span data-stu-id="5e409-143">You can't delete a blob with snapshots unless hello snapshots are also deleted.</span></span> <span data-ttu-id="5e409-144">Du kan ta bort en ögonblicksbild individuellt eller ange att alla ögonblicksbilder tas bort när hello källa blob tas bort.</span><span class="sxs-lookup"><span data-stu-id="5e409-144">You can delete a snapshot individually, or specify that all snapshots be deleted when hello source blob is deleted.</span></span> <span data-ttu-id="5e409-145">Om du försöker toodelete en blob som fortfarande har ögonblicksbilder, uppstår ett fel.</span><span class="sxs-lookup"><span data-stu-id="5e409-145">If you attempt toodelete a blob that still has snapshots, an error results.</span></span>

<span data-ttu-id="5e409-146">Hej följande exempel visas hur toodelete en blob och dess ögonblicksbilder i .NET, där `blockBlob` är ett objekt av typen [CloudBlockBlob][dotnet_CloudBlockBlob]:</span><span class="sxs-lookup"><span data-stu-id="5e409-146">hello following code example shows how toodelete a blob and its snapshots in .NET, where `blockBlob` is an object of type [CloudBlockBlob][dotnet_CloudBlockBlob]:</span></span>

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a><span data-ttu-id="5e409-147">Ögonblicksbilder med Azure Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="5e409-147">Snapshots with Azure Premium Storage</span></span>
<span data-ttu-id="5e409-148">När du använder ögonblicksbilder med Premium-lagring, tillämpa hello följande regler:</span><span class="sxs-lookup"><span data-stu-id="5e409-148">When using snapshots with Premium Storage, hello following rules apply:</span></span>

* <span data-ttu-id="5e409-149">hello maximalt antal ögonblicksbilder per sidblob i ett premiumlagringskonto är 100.</span><span class="sxs-lookup"><span data-stu-id="5e409-149">hello maximum number of snapshots per page blob in a premium storage account is 100.</span></span> <span data-ttu-id="5e409-150">Om denna gräns har överskridits hello ögonblicksbild Blob-åtgärd returnerar en felkod 409 (`SnapshotCountExceeded`).</span><span class="sxs-lookup"><span data-stu-id="5e409-150">If that limit is exceeded, hello Snapshot Blob operation returns error code 409 (`SnapshotCountExceeded`).</span></span>
* <span data-ttu-id="5e409-151">Du kan ta en ögonblicksbild av en sidblob i ett premiumlagringskonto var 10: e minut.</span><span class="sxs-lookup"><span data-stu-id="5e409-151">You can take a snapshot of a page blob in a premium storage account once every 10 minutes.</span></span> <span data-ttu-id="5e409-152">Om denna kurs har överskridits hello ögonblicksbild Blob-åtgärd returnerar en felkod 409 (`SnapshotOperationRateExceeded`).</span><span class="sxs-lookup"><span data-stu-id="5e409-152">If that rate is exceeded, hello Snapshot Blob operation returns error code 409 (`SnapshotOperationRateExceeded`).</span></span>
* <span data-ttu-id="5e409-153">tooread en ögonblicksbild, du kan använda hello kopiera Blob åtgärden toocopy en ögonblicksbild tooanother sidblob i hello-konto.</span><span class="sxs-lookup"><span data-stu-id="5e409-153">tooread a snapshot, you can use hello Copy Blob operation toocopy a snapshot tooanother page blob in hello account.</span></span> <span data-ttu-id="5e409-154">hello mål-blob för hello kopieringsåtgärden får inte ha några befintliga ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="5e409-154">hello destination blob for hello copy operation must not have any existing snapshots.</span></span> <span data-ttu-id="5e409-155">Om hello mål-blob har ögonblicksbilder, hello kopiera Blob-åtgärd och returnerar sedan felkoden 409 (`SnapshotsPresent`).</span><span class="sxs-lookup"><span data-stu-id="5e409-155">If hello destination blob does have snapshots, then hello Copy Blob operation returns error code 409 (`SnapshotsPresent`).</span></span>

## <a name="return-hello-absolute-uri-tooa-snapshot"></a><span data-ttu-id="5e409-156">Returnera hello absolut URI tooa ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="5e409-156">Return hello absolute URI tooa snapshot</span></span>
<span data-ttu-id="5e409-157">Den här C#-kodexempel skapar en ögonblicksbild och skriver ut hello absolut URI för hello primär plats.</span><span class="sxs-lookup"><span data-stu-id="5e409-157">This C# code example creates a snapshot and writes out hello absolute URI for hello primary location.</span></span>

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

## <a name="understand-how-snapshots-accrue-charges"></a><span data-ttu-id="5e409-158">Förstå hur ögonblicksbilder påförs kostnader</span><span class="sxs-lookup"><span data-stu-id="5e409-158">Understand how snapshots accrue charges</span></span>
<span data-ttu-id="5e409-159">Skapa en ögonblicksbild som är en skrivskyddad kopia av en blob, kan leda till ytterligare data lagringskonto avgifter tooyour.</span><span class="sxs-lookup"><span data-stu-id="5e409-159">Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges tooyour account.</span></span> <span data-ttu-id="5e409-160">När du skapar ditt program, är det viktigt toobe som är medvetna om hur dessa avgifter kan tillkomma så att du kan minimera kostnaderna.</span><span class="sxs-lookup"><span data-stu-id="5e409-160">When designing your application, it is important toobe aware of how these charges might accrue so that you can minimize costs.</span></span>

### <a name="important-billing-considerations"></a><span data-ttu-id="5e409-161">Tänk på följande fakturering</span><span class="sxs-lookup"><span data-stu-id="5e409-161">Important billing considerations</span></span>
<span data-ttu-id="5e409-162">hello följande lista innehåller nyckeln pekar tooconsider när du skapar en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-162">hello following list includes key points tooconsider when creating a snapshot.</span></span>

* <span data-ttu-id="5e409-163">Ditt lagringskonto debiteras för unik block eller sidor, oavsett om de är i hello blob eller hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-163">Your storage account incurs charges for unique blocks or pages, whether they are in hello blob or in hello snapshot.</span></span> <span data-ttu-id="5e409-164">Ditt konto leder inte till ytterligare avgifter för ögonblicksbilder kopplade till en blob förrän du har uppdaterat hello blob som de är baserad på.</span><span class="sxs-lookup"><span data-stu-id="5e409-164">Your account does not incur additional charges for snapshots associated with a blob until you update hello blob on which they are based.</span></span> <span data-ttu-id="5e409-165">När du har uppdaterat hello grundläggande blob skiljer sig från dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="5e409-165">After you update hello base blob, it diverges from its snapshots.</span></span> <span data-ttu-id="5e409-166">När detta inträffar debiteras du för hello unika block eller sidor i varje blob eller en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-166">When this happens, you are charged for hello unique blocks or pages in each blob or snapshot.</span></span>
* <span data-ttu-id="5e409-167">När du ersätter ett block inom en blockblobb debiteras som blockerar senare som en unik block.</span><span class="sxs-lookup"><span data-stu-id="5e409-167">When you replace a block within a block blob, that block is subsequently charged as a unique block.</span></span> <span data-ttu-id="5e409-168">Detta gäller även om hello block har hello samma blockera ID och hello samma data som den har i hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-168">This is true even if hello block has hello same block ID and hello same data as it has in hello snapshot.</span></span> <span data-ttu-id="5e409-169">När hello block strävar igen och det avviker från sin motsvarighet i en ögonblicksbild och du kommer att debiteras för sina data.</span><span class="sxs-lookup"><span data-stu-id="5e409-169">After hello block is committed again, it diverges from its counterpart in any snapshot, and you will be charged for its data.</span></span> <span data-ttu-id="5e409-170">hello detsamma gäller för en sida i en sidblob som uppdateras med identiska data.</span><span class="sxs-lookup"><span data-stu-id="5e409-170">hello same holds true for a page in a page blob that's updated with identical data.</span></span>
* <span data-ttu-id="5e409-171">Ersätta en blockblobb genom att anropa hello [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], eller [UploadFromByteArray] [ dotnet_UploadFromByteArray] metoden ersätter alla block i hello-blob.</span><span class="sxs-lookup"><span data-stu-id="5e409-171">Replacing a block blob by calling hello [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] method replaces all blocks in hello blob.</span></span> <span data-ttu-id="5e409-172">Om du har en ögonblicksbild som är associerade med blobben alla block i grundläggande hello-blob och ögonblicksbild nu avvika och du kommer att debiteras för alla hello block i båda blobbar.</span><span class="sxs-lookup"><span data-stu-id="5e409-172">If you have a snapshot associated with that blob, all blocks in hello base blob and snapshot now diverge, and you will be charged for all hello blocks in both blobs.</span></span> <span data-ttu-id="5e409-173">Detta gäller även om hello data i grundläggande hello-blob och hello ögonblicksbild vara identiska.</span><span class="sxs-lookup"><span data-stu-id="5e409-173">This is true even if hello data in hello base blob and hello snapshot remain identical.</span></span>
* <span data-ttu-id="5e409-174">hello Azure Blob-tjänsten har inte en innebär toodetermine om två block innehåller samma data.</span><span class="sxs-lookup"><span data-stu-id="5e409-174">hello Azure Blob service does not have a means toodetermine whether two blocks contain identical data.</span></span> <span data-ttu-id="5e409-175">Varje block som har överförts och verkställs behandlas som unika, även om den har hello samma data och hello samma blockera ID.</span><span class="sxs-lookup"><span data-stu-id="5e409-175">Each block that is uploaded and committed is treated as unique, even if it has hello same data and hello same block ID.</span></span> <span data-ttu-id="5e409-176">Eftersom det tillkomma avgifter för unik block, är det viktigt tooconsider som uppdaterar en blob som har en ögonblicksbild resulterar i ytterligare unika block och ytterligare kostnader.</span><span class="sxs-lookup"><span data-stu-id="5e409-176">Because charges accrue for unique blocks, it's important tooconsider that updating a blob that has a snapshot results in additional unique blocks and additional charges.</span></span>

### <a name="minimize-cost-with-snapshot-management"></a><span data-ttu-id="5e409-177">Minimera kostnad med hantering av ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="5e409-177">Minimize cost with snapshot management</span></span>

<span data-ttu-id="5e409-178">Vi rekommenderar att du hanterar din ögonblicksbilder noggrant tooavoid extra kostnader.</span><span class="sxs-lookup"><span data-stu-id="5e409-178">We recommend managing your snapshots carefully tooavoid extra charges.</span></span> <span data-ttu-id="5e409-179">Du kan följa dessa rekommendationer toohelp minimera hello kostnader hello lagring av ögonblicksbilder av din:</span><span class="sxs-lookup"><span data-stu-id="5e409-179">You can follow these best practices toohelp minimize hello costs incurred by hello storage of your snapshots:</span></span>

* <span data-ttu-id="5e409-180">Ta bort och återskapa ögonblicksbilder kopplade till en blob när du uppdaterar hello blob, även om du uppdaterar med identiska data om din program-designen kräver att du behåller ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="5e409-180">Delete and re-create snapshots associated with a blob whenever you update hello blob, even if you are updating with identical data, unless your application design requires that you maintain snapshots.</span></span> <span data-ttu-id="5e409-181">Genom att ta bort och återskapa hello blob ögonblicksbilder, kan du se till att hello blob och ögonblicksbilder inte sig.</span><span class="sxs-lookup"><span data-stu-id="5e409-181">By deleting and re-creating hello blob's snapshots, you can ensure that hello blob and snapshots do not diverge.</span></span>
* <span data-ttu-id="5e409-182">Om du underhåller ögonblicksbilder för en blob och undvika anropar [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], eller [UploadFromByteArray] [ dotnet_UploadFromByteArray] tooupdate hello blob.</span><span class="sxs-lookup"><span data-stu-id="5e409-182">If you are maintaining snapshots for a blob, avoid calling [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] tooupdate hello blob.</span></span> <span data-ttu-id="5e409-183">Dessa metoder ersätta alla hello block i hello blob, orsakar din grundläggande blob och dess ögonblicksbilder toodiverge avsevärt.</span><span class="sxs-lookup"><span data-stu-id="5e409-183">These methods replace all of hello blocks in hello blob, causing your base blob and its snapshots toodiverge significantly.</span></span> <span data-ttu-id="5e409-184">I stället uppdatera hello lägsta möjliga antal block med hjälp av hello [PutBlock] [ dotnet_PutBlock] och [PutBlockList] [ dotnet_PutBlockList] metoder.</span><span class="sxs-lookup"><span data-stu-id="5e409-184">Instead, update hello fewest possible number of blocks by using hello [PutBlock][dotnet_PutBlock] and [PutBlockList][dotnet_PutBlockList] methods.</span></span>

### <a name="snapshot-billing-scenarios"></a><span data-ttu-id="5e409-185">Ögonblicksbild fakturering scenarier</span><span class="sxs-lookup"><span data-stu-id="5e409-185">Snapshot billing scenarios</span></span>
<span data-ttu-id="5e409-186">hello följande scenarier visas hur debiteringar görs för en blockblobb och dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="5e409-186">hello following scenarios demonstrate how charges accrue for a block blob and its snapshots.</span></span>

<span data-ttu-id="5e409-187">**Scenario 1**</span><span class="sxs-lookup"><span data-stu-id="5e409-187">**Scenario 1**</span></span>

<span data-ttu-id="5e409-188">I scenario 1 har grundläggande hello-blob inte uppdaterats efter hello ögonblicksbilden togs, så avgifter tas ut endast unika block 1, 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="5e409-188">In scenario 1, hello base blob has not been updated after hello snapshot was taken, so charges are incurred only for unique blocks 1, 2, and 3.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

<span data-ttu-id="5e409-190">**Scenario 2**</span><span class="sxs-lookup"><span data-stu-id="5e409-190">**Scenario 2**</span></span>

<span data-ttu-id="5e409-191">I scenario 2 grundläggande hello-blob har uppdaterats, men hello ögonblicksbild har inte.</span><span class="sxs-lookup"><span data-stu-id="5e409-191">In scenario 2, hello base blob has been updated, but hello snapshot has not.</span></span> <span data-ttu-id="5e409-192">Blockera 3 har uppdaterats och även om den innehåller hello samma data och hello samma ID kan inte hello samma som blockerar 3 i hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5e409-192">Block 3 was updated, and even though it contains hello same data and hello same ID, it is not hello same as block 3 in hello snapshot.</span></span> <span data-ttu-id="5e409-193">Därför debiteras hello kontot för fyra block.</span><span class="sxs-lookup"><span data-stu-id="5e409-193">As a result, hello account is charged for four blocks.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

<span data-ttu-id="5e409-195">**Scenario 3**</span><span class="sxs-lookup"><span data-stu-id="5e409-195">**Scenario 3**</span></span>

<span data-ttu-id="5e409-196">I scenario 3 grundläggande hello-blob har uppdaterats, men hello ögonblicksbild har inte.</span><span class="sxs-lookup"><span data-stu-id="5e409-196">In scenario 3, hello base blob has been updated, but hello snapshot has not.</span></span> <span data-ttu-id="5e409-197">Block 3 ersattes med block 4 i grundläggande hello-blob, men visar hello ögonblicksbild block 3.</span><span class="sxs-lookup"><span data-stu-id="5e409-197">Block 3 was replaced with block 4 in hello base blob, but hello snapshot still reflects block 3.</span></span> <span data-ttu-id="5e409-198">Därför debiteras hello kontot för fyra block.</span><span class="sxs-lookup"><span data-stu-id="5e409-198">As a result, hello account is charged for four blocks.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

<span data-ttu-id="5e409-200">**Scenario 4**</span><span class="sxs-lookup"><span data-stu-id="5e409-200">**Scenario 4**</span></span>

<span data-ttu-id="5e409-201">I scenariot 4 grundläggande hello-blob har uppdaterats helt och innehåller inga av dess ursprungliga block.</span><span class="sxs-lookup"><span data-stu-id="5e409-201">In scenario 4, hello base blob has been completely updated and contains none of its original blocks.</span></span> <span data-ttu-id="5e409-202">Därför debiteras hello kontot för alla åtta unika block.</span><span class="sxs-lookup"><span data-stu-id="5e409-202">As a result, hello account is charged for all eight unique blocks.</span></span> <span data-ttu-id="5e409-203">Den här situationen kan uppstå om du använder en metod för att uppdatera som [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], eller [UploadFromByteArray][dotnet_UploadFromByteArray], eftersom dessa metoder ersätta alla hello innehållet i en blob.</span><span class="sxs-lookup"><span data-stu-id="5e409-203">This scenario can occur if you are using an update method such as [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray], because these methods replace all of hello contents of a blob.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a><span data-ttu-id="5e409-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e409-205">Next steps</span></span>

* <span data-ttu-id="5e409-206">Du hittar mer information om hur du arbetar med ögonblicksbilder av virtuella datorer (VM)-disk i [säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder](storage-incremental-snapshots.md)</span><span class="sxs-lookup"><span data-stu-id="5e409-206">You can find more information about working with virtual machine (VM) disk snapshots in [Back up Azure unmanaged VM disks with incremental snapshots](storage-incremental-snapshots.md)</span></span>

* <span data-ttu-id="5e409-207">Ytterligare kodexempel med Blob storage finns [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span><span class="sxs-lookup"><span data-stu-id="5e409-207">For additional code examples using Blob storage, see [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span></span> <span data-ttu-id="5e409-208">Du kan hämta ett exempelprogram och kör den, eller bläddra hello koden på GitHub.</span><span class="sxs-lookup"><span data-stu-id="5e409-208">You can download a sample application and run it, or browse hello code on GitHub.</span></span>

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