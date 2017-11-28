---
title: "Skapa en skrivskyddad ögonblicksbild av en blobb i Azure Storage | Microsoft Docs"
description: "Lär dig hur du skapar en ögonblicksbild av en blob att säkerhetskopiera blob-data vid en given tidpunkt. Förstå hur ögonblicksbilder debiteras och hur du använder dem för att minimera kapacitet avgifter."
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
ms.openlocfilehash: 6ebb77f4ec5f1887e5c55bf1c8c64224a9233654
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-blob-snapshot"></a><span data-ttu-id="968ca-104">Skapa en blob-ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="968ca-104">Create a blob snapshot</span></span>

<span data-ttu-id="968ca-105">En ögonblicksbild är en skrivskyddad version av en blob som utförs i en punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="968ca-105">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="968ca-106">Ögonblicksbilder är användbara för att säkerhetskopiera blobbar.</span><span class="sxs-lookup"><span data-stu-id="968ca-106">Snapshots are useful for backing up blobs.</span></span> <span data-ttu-id="968ca-107">När du har skapat en ögonblicksbild, läsa, kopiera eller ta bort den, men du kan inte ändra den.</span><span class="sxs-lookup"><span data-stu-id="968ca-107">After you create a snapshot, you can read, copy, or delete it, but you cannot modify it.</span></span>

<span data-ttu-id="968ca-108">En ögonblicksbild av en blob är identisk med dess grundläggande blob, förutom att blob-URI: N har en **DateTime** värde som läggs till blob-URI om du vill ange när ögonblicksbilden togs.</span><span class="sxs-lookup"><span data-stu-id="968ca-108">A snapshot of a blob is identical to its base blob, except that the blob URI has a **DateTime** value appended to the blob URI to indicate the time at which the snapshot was taken.</span></span> <span data-ttu-id="968ca-109">Till exempel om en sida blob-URI är `http://storagesample.core.blob.windows.net/mydrives/myvhd`, ögonblicksbilden URI liknar `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span><span class="sxs-lookup"><span data-stu-id="968ca-109">For example, if a page blob URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, the snapshot URI is similar to `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span></span>

> [!NOTE]
> <span data-ttu-id="968ca-110">Alla ögonblicksbilder dela grundläggande blob-URI.</span><span class="sxs-lookup"><span data-stu-id="968ca-110">All snapshots share the base blob's URI.</span></span> <span data-ttu-id="968ca-111">Är den enda skillnaden mellan grundläggande blob och ögonblicksbilden den tillagda **DateTime** värde.</span><span class="sxs-lookup"><span data-stu-id="968ca-111">The only distinction between the base blob and the snapshot is the appended **DateTime** value.</span></span>
>

<span data-ttu-id="968ca-112">En blob kan ha valfritt antal ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="968ca-112">A blob can have any number of snapshots.</span></span> <span data-ttu-id="968ca-113">Ögonblicksbilder sparas tills de uttryckligen tas bort.</span><span class="sxs-lookup"><span data-stu-id="968ca-113">Snapshots persist until they are explicitly deleted.</span></span> <span data-ttu-id="968ca-114">En ögonblicksbild inte sträcker sig längre än dess grundläggande blob.</span><span class="sxs-lookup"><span data-stu-id="968ca-114">A snapshot cannot outlive its base blob.</span></span> <span data-ttu-id="968ca-115">Du kan räkna upp ögonblicksbilder kopplade till grundläggande blob att spåra din aktuella ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="968ca-115">You can enumerate the snapshots associated with the base blob to track your current snapshots.</span></span>

<span data-ttu-id="968ca-116">När du skapar en ögonblicksbild av en blob kopieras blobens Systemegenskaper till ögonblicksbilden med samma värden.</span><span class="sxs-lookup"><span data-stu-id="968ca-116">When you create a snapshot of a blob, the blob's system properties are copied to the snapshot with the same values.</span></span> <span data-ttu-id="968ca-117">Grundläggande blob metadata kopieras också till ögonblicksbilden, om du inte anger separat metadata för ögonblicksbilden när du skapar den.</span><span class="sxs-lookup"><span data-stu-id="968ca-117">The base blob's metadata is also copied to the snapshot, unless you specify separate metadata for the snapshot when you create it.</span></span>

<span data-ttu-id="968ca-118">Alla lån som är associerade med grundläggande blob påverkar inte ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="968ca-118">Any leases associated with the base blob do not affect the snapshot.</span></span> <span data-ttu-id="968ca-119">Du kan inte hämta ett lån på en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="968ca-119">You cannot acquire a lease on a snapshot.</span></span>

<span data-ttu-id="968ca-120">En VHD-fil används för att spara aktuella information och status för en virtuell disk.</span><span class="sxs-lookup"><span data-stu-id="968ca-120">A VHD file is used to store the current information and status for a VM disk.</span></span> <span data-ttu-id="968ca-121">Du kan koppla bort en disk från den virtuella datorn eller stänga av den virtuella datorn och sedan ta en ögonblicksbild av dess VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="968ca-121">You can detach a disk from within the VM or shut down the VM, and then take a snapshot of its VHD file.</span></span> <span data-ttu-id="968ca-122">Du kan använda ögonblicksbild filen senare för att hämta VHD-filen då i tid och återskapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="968ca-122">You can use that snapshot file later to retrieve the VHD file at that point in time and recreate the VM.</span></span>

<span data-ttu-id="968ca-123">Om kryptering för lagring-tjänsten (SSE) är aktiverat för det lagringskonto där blobben finns, krypteras alla ögonblicksbilder av blobben i vila.</span><span class="sxs-lookup"><span data-stu-id="968ca-123">If Storage Service Encryption (SSE) is enabled for the storage account in which the blob resides, then any snapshots taken of that blob will be encrypted at rest.</span></span>

## <a name="create-a-snapshot"></a><span data-ttu-id="968ca-124">Skapa en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="968ca-124">Create a snapshot</span></span>
<span data-ttu-id="968ca-125">Följande kodexempel visar hur du skapar en ögonblicksbild med hjälp av den [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="968ca-125">The following code example shows how to create a snapshot by using the [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span> <span data-ttu-id="968ca-126">Det här exemplet anger ytterligare metadata för ögonblicksbilden när den skapas.</span><span class="sxs-lookup"><span data-stu-id="968ca-126">This example specifies additional metadata for the snapshot when it is created.</span></span>

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
        // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
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

## <a name="copy-snapshots"></a><span data-ttu-id="968ca-127">Kopiera ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="968ca-127">Copy snapshots</span></span>
<span data-ttu-id="968ca-128">Kopieringsåtgärder som rör blobbar och ögonblicksbilder följer dessa regler:</span><span class="sxs-lookup"><span data-stu-id="968ca-128">Copy operations involving blobs and snapshots follow these rules:</span></span>

* <span data-ttu-id="968ca-129">Du kan kopiera en ögonblicksbild över dess grundläggande blob.</span><span class="sxs-lookup"><span data-stu-id="968ca-129">You can copy a snapshot over its base blob.</span></span> <span data-ttu-id="968ca-130">Du kan återställa en tidigare version av en blob genom att befordra en ögonblicksbild till positionen för den grundläggande blobben.</span><span class="sxs-lookup"><span data-stu-id="968ca-130">By promoting a snapshot to the position of the base blob, you can restore an earlier version of a blob.</span></span> <span data-ttu-id="968ca-131">Ögonblicksbild fortfarande, men grundläggande blob skrivs över med en skrivbar kopia av ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="968ca-131">The snapshot remains, but the base blob is overwritten with a writable copy of the snapshot.</span></span>
* <span data-ttu-id="968ca-132">Du kan kopiera en ögonblicksbild till en mål-blob med ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="968ca-132">You can copy a snapshot to a destination blob with a different name.</span></span> <span data-ttu-id="968ca-133">Det resulterande mål-blobben är en skrivbar blob och inte en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="968ca-133">The resulting destination blob is a writable blob and not a snapshot.</span></span>
* <span data-ttu-id="968ca-134">När en käll-blob kopieras kopieras alla ögonblicksbilder på käll-blob inte till målet.</span><span class="sxs-lookup"><span data-stu-id="968ca-134">When a source blob is copied, any snapshots of the source blob are not copied to the destination.</span></span> <span data-ttu-id="968ca-135">När en mål-blob skrivs över med en kopia, bevaras eventuella ögonblicksbilder som är kopplade till den ursprungliga mål-blobben.</span><span class="sxs-lookup"><span data-stu-id="968ca-135">When a destination blob is overwritten with a copy, any snapshots associated with the original destination blob remain intact.</span></span>
* <span data-ttu-id="968ca-136">När du skapar en ögonblicksbild av en blockblobb kopieras även blobens allokerat Blockeringslista till ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="968ca-136">When you create a snapshot of a block blob, the blob's committed block list is also copied to the snapshot.</span></span> <span data-ttu-id="968ca-137">Alla ogenomförda block kopieras inte.</span><span class="sxs-lookup"><span data-stu-id="968ca-137">Any uncommitted blocks are not copied.</span></span>

## <a name="specify-an-access-condition"></a><span data-ttu-id="968ca-138">Ange ett villkor för åtkomst</span><span class="sxs-lookup"><span data-stu-id="968ca-138">Specify an access condition</span></span>
<span data-ttu-id="968ca-139">När du anropar [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], du kan ange ett villkor för åtkomst så att ögonblicksbilden skapas endast om ett villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="968ca-139">When you call [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], you can specify an access condition so that the snapshot is created only if a condition is met.</span></span> <span data-ttu-id="968ca-140">Om du vill ange ett villkor för åtkomst av [AccessCondition] [ dotnet_AccessCondition] parameter.</span><span class="sxs-lookup"><span data-stu-id="968ca-140">To specify an access condition, use the [AccessCondition][dotnet_AccessCondition] parameter.</span></span> <span data-ttu-id="968ca-141">Om det angivna villkoret uppfylls inte ögonblicksbilden har inte skapats och Blob-tjänsten returnerar statuskod [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.</span><span class="sxs-lookup"><span data-stu-id="968ca-141">If the specified condition is not met, the snapshot is not created, and the Blob service returns status code [HTTPStatusCode][dotnet_HTTPStatusCode].PreconditionFailed.</span></span>

## <a name="delete-snapshots"></a><span data-ttu-id="968ca-142">Ta bort ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="968ca-142">Delete snapshots</span></span>
<span data-ttu-id="968ca-143">Du kan inte ta bort en blob med ögonblicksbilder om ögonblicksbilderna tas också bort.</span><span class="sxs-lookup"><span data-stu-id="968ca-143">You can't delete a blob with snapshots unless the snapshots are also deleted.</span></span> <span data-ttu-id="968ca-144">Du kan ta bort en ögonblicksbild individuellt eller ange att alla ögonblicksbilder tas bort när käll-blob har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="968ca-144">You can delete a snapshot individually, or specify that all snapshots be deleted when the source blob is deleted.</span></span> <span data-ttu-id="968ca-145">Om du försöker ta bort en blobb som fortfarande har ögonblicksbilder, uppstår ett fel.</span><span class="sxs-lookup"><span data-stu-id="968ca-145">If you attempt to delete a blob that still has snapshots, an error results.</span></span>

<span data-ttu-id="968ca-146">Följande kodexempel visar hur du tar bort en blob och dess ögonblicksbilder i .NET, där `blockBlob` är ett objekt av typen [CloudBlockBlob][dotnet_CloudBlockBlob]:</span><span class="sxs-lookup"><span data-stu-id="968ca-146">The following code example shows how to delete a blob and its snapshots in .NET, where `blockBlob` is an object of type [CloudBlockBlob][dotnet_CloudBlockBlob]:</span></span>

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a><span data-ttu-id="968ca-147">Ögonblicksbilder med Azure Premium-lagring</span><span class="sxs-lookup"><span data-stu-id="968ca-147">Snapshots with Azure Premium Storage</span></span>
<span data-ttu-id="968ca-148">När du använder ögonblicksbilder med Premium-lagring, gäller följande regler:</span><span class="sxs-lookup"><span data-stu-id="968ca-148">When using snapshots with Premium Storage, the following rules apply:</span></span>

* <span data-ttu-id="968ca-149">Det maximala antalet ögonblicksbilder per sidblob i ett premiumlagringskonto är 100.</span><span class="sxs-lookup"><span data-stu-id="968ca-149">The maximum number of snapshots per page blob in a premium storage account is 100.</span></span> <span data-ttu-id="968ca-150">Om denna gräns har överskridits ögonblicksbild Blob-åtgärd returnerar felkoden 409 (`SnapshotCountExceeded`).</span><span class="sxs-lookup"><span data-stu-id="968ca-150">If that limit is exceeded, the Snapshot Blob operation returns error code 409 (`SnapshotCountExceeded`).</span></span>
* <span data-ttu-id="968ca-151">Du kan ta en ögonblicksbild av en sidblob i ett premiumlagringskonto var 10: e minut.</span><span class="sxs-lookup"><span data-stu-id="968ca-151">You can take a snapshot of a page blob in a premium storage account once every 10 minutes.</span></span> <span data-ttu-id="968ca-152">Om denna kurs har överskridits ögonblicksbild Blob-åtgärd returnerar felkoden 409 (`SnapshotOperationRateExceeded`).</span><span class="sxs-lookup"><span data-stu-id="968ca-152">If that rate is exceeded, the Snapshot Blob operation returns error code 409 (`SnapshotOperationRateExceeded`).</span></span>
* <span data-ttu-id="968ca-153">Du kan använda kopiera Blob-åtgärden för att kopiera en ögonblicksbild till en annan sidblob i kontot för att läsa en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="968ca-153">To read a snapshot, you can use the Copy Blob operation to copy a snapshot to another page blob in the account.</span></span> <span data-ttu-id="968ca-154">Mål-blob för kopieringen får inte ha några befintliga ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="968ca-154">The destination blob for the copy operation must not have any existing snapshots.</span></span> <span data-ttu-id="968ca-155">Om mål-blob har ögonblicksbilder och sedan kopiera Blob-åtgärd returnerar felkoden 409 (`SnapshotsPresent`).</span><span class="sxs-lookup"><span data-stu-id="968ca-155">If the destination blob does have snapshots, then the Copy Blob operation returns error code 409 (`SnapshotsPresent`).</span></span>

## <a name="return-the-absolute-uri-to-a-snapshot"></a><span data-ttu-id="968ca-156">Returnera en absolut URI till en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="968ca-156">Return the absolute URI to a snapshot</span></span>
<span data-ttu-id="968ca-157">Den här C#-kodexempel skapar en ögonblicksbild och skriver ut absolut URI för den primära platsen.</span><span class="sxs-lookup"><span data-stu-id="968ca-157">This C# code example creates a snapshot and writes out the absolute URI for the primary location.</span></span>

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a><span data-ttu-id="968ca-158">Förstå hur ögonblicksbilder påförs kostnader</span><span class="sxs-lookup"><span data-stu-id="968ca-158">Understand how snapshots accrue charges</span></span>
<span data-ttu-id="968ca-159">Skapa en ögonblicksbild som är en skrivskyddad kopia av en blob, kan leda till ytterligare data lagringskostnader för volymer till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="968ca-159">Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges to your account.</span></span> <span data-ttu-id="968ca-160">När du skapar ditt program, är det viktigt att känna till hur dessa avgifter kan tillkomma så att du kan minimera kostnaderna.</span><span class="sxs-lookup"><span data-stu-id="968ca-160">When designing your application, it is important to be aware of how these charges might accrue so that you can minimize costs.</span></span>

### <a name="important-billing-considerations"></a><span data-ttu-id="968ca-161">Tänk på följande fakturering</span><span class="sxs-lookup"><span data-stu-id="968ca-161">Important billing considerations</span></span>
<span data-ttu-id="968ca-162">Följande lista innehåller viktiga saker att tänka på när du skapar en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="968ca-162">The following list includes key points to consider when creating a snapshot.</span></span>

* <span data-ttu-id="968ca-163">Ditt lagringskonto debiteras för unik block eller sidor, oavsett om de är i blob eller i ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="968ca-163">Your storage account incurs charges for unique blocks or pages, whether they are in the blob or in the snapshot.</span></span> <span data-ttu-id="968ca-164">Ditt konto leder inte till ytterligare avgifter för ögonblicksbilder kopplade till en blob förrän du har uppdaterat blob som de är baserad på.</span><span class="sxs-lookup"><span data-stu-id="968ca-164">Your account does not incur additional charges for snapshots associated with a blob until you update the blob on which they are based.</span></span> <span data-ttu-id="968ca-165">När du har uppdaterat grundläggande blob skiljer sig från dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="968ca-165">After you update the base blob, it diverges from its snapshots.</span></span> <span data-ttu-id="968ca-166">När detta inträffar debiteras du för unik block eller sidor i varje blob eller en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="968ca-166">When this happens, you are charged for the unique blocks or pages in each blob or snapshot.</span></span>
* <span data-ttu-id="968ca-167">När du ersätter ett block inom en blockblobb debiteras som blockerar senare som en unik block.</span><span class="sxs-lookup"><span data-stu-id="968ca-167">When you replace a block within a block blob, that block is subsequently charged as a unique block.</span></span> <span data-ttu-id="968ca-168">Detta gäller även om blocket har samma block-ID och samma data eftersom den har i ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="968ca-168">This is true even if the block has the same block ID and the same data as it has in the snapshot.</span></span> <span data-ttu-id="968ca-169">När blocket strävar igen och det avviker från sin motsvarighet i en ögonblicksbild och du kommer att debiteras för sina data.</span><span class="sxs-lookup"><span data-stu-id="968ca-169">After the block is committed again, it diverges from its counterpart in any snapshot, and you will be charged for its data.</span></span> <span data-ttu-id="968ca-170">Detsamma gäller för en sida i en sidblob som uppdateras med identiska data.</span><span class="sxs-lookup"><span data-stu-id="968ca-170">The same holds true for a page in a page blob that's updated with identical data.</span></span>
* <span data-ttu-id="968ca-171">Ersätta en blockblobb genom att anropa den [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], eller [UploadFromByteArray] [ dotnet_UploadFromByteArray] metoden ersätter alla block i blob.</span><span class="sxs-lookup"><span data-stu-id="968ca-171">Replacing a block blob by calling the [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] method replaces all blocks in the blob.</span></span> <span data-ttu-id="968ca-172">Om du har en ögonblicksbild som är associerade med blobben alla block i grundläggande blob och ögonblicksbild nu avvika och du kommer att debiteras för alla block i båda blobbar.</span><span class="sxs-lookup"><span data-stu-id="968ca-172">If you have a snapshot associated with that blob, all blocks in the base blob and snapshot now diverge, and you will be charged for all the blocks in both blobs.</span></span> <span data-ttu-id="968ca-173">Detta gäller även om data i grundläggande blob och ögonblicksbilden vara identiska.</span><span class="sxs-lookup"><span data-stu-id="968ca-173">This is true even if the data in the base blob and the snapshot remain identical.</span></span>
* <span data-ttu-id="968ca-174">Azure Blob-tjänsten har inte ett sätt att avgöra om två block innehåller identiska data.</span><span class="sxs-lookup"><span data-stu-id="968ca-174">The Azure Blob service does not have a means to determine whether two blocks contain identical data.</span></span> <span data-ttu-id="968ca-175">Varje block som har överförts och verkställs behandlas som unika, även om den har samma data och samma block-ID.</span><span class="sxs-lookup"><span data-stu-id="968ca-175">Each block that is uploaded and committed is treated as unique, even if it has the same data and the same block ID.</span></span> <span data-ttu-id="968ca-176">Eftersom det tillkomma avgifter för unik block, är det viktigt att beakta som uppdaterar en blob som har en ögonblicksbild resulterar i ytterligare unika block och ytterligare avgifter.</span><span class="sxs-lookup"><span data-stu-id="968ca-176">Because charges accrue for unique blocks, it's important to consider that updating a blob that has a snapshot results in additional unique blocks and additional charges.</span></span>

### <a name="minimize-cost-with-snapshot-management"></a><span data-ttu-id="968ca-177">Minimera kostnad med hantering av ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="968ca-177">Minimize cost with snapshot management</span></span>

<span data-ttu-id="968ca-178">Vi rekommenderar att du hanterar din ögonblicksbilder noggrant för att undvika extra kostnader.</span><span class="sxs-lookup"><span data-stu-id="968ca-178">We recommend managing your snapshots carefully to avoid extra charges.</span></span> <span data-ttu-id="968ca-179">Du kan följa dessa rekommendationer för att minimera kostnaderna genom lagring av ögonblicksbilder av din:</span><span class="sxs-lookup"><span data-stu-id="968ca-179">You can follow these best practices to help minimize the costs incurred by the storage of your snapshots:</span></span>

* <span data-ttu-id="968ca-180">Ta bort och återskapa ögonblicksbilder kopplade till en blob när du uppdaterar blob, även om du uppdaterar med identiska data om din program-designen kräver att du behåller ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="968ca-180">Delete and re-create snapshots associated with a blob whenever you update the blob, even if you are updating with identical data, unless your application design requires that you maintain snapshots.</span></span> <span data-ttu-id="968ca-181">Genom att ta bort och återskapa det blob ögonblicksbilder, kan du se till att blob och ögonblicksbilder inte sig.</span><span class="sxs-lookup"><span data-stu-id="968ca-181">By deleting and re-creating the blob's snapshots, you can ensure that the blob and snapshots do not diverge.</span></span>
* <span data-ttu-id="968ca-182">Om du underhåller ögonblicksbilder för en blob och undvika anropar [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], eller [UploadFromByteArray] [ dotnet_UploadFromByteArray] att uppdatera blob.</span><span class="sxs-lookup"><span data-stu-id="968ca-182">If you are maintaining snapshots for a blob, avoid calling [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] to update the blob.</span></span> <span data-ttu-id="968ca-183">Dessa metoder ersätta alla block i blob orsakar din grundläggande blob och dess ögonblicksbilder till avvika avsevärt.</span><span class="sxs-lookup"><span data-stu-id="968ca-183">These methods replace all of the blocks in the blob, causing your base blob and its snapshots to diverge significantly.</span></span> <span data-ttu-id="968ca-184">Uppdatera i stället det minsta antalet block med hjälp av den [PutBlock] [ dotnet_PutBlock] och [PutBlockList] [ dotnet_PutBlockList] metoder.</span><span class="sxs-lookup"><span data-stu-id="968ca-184">Instead, update the fewest possible number of blocks by using the [PutBlock][dotnet_PutBlock] and [PutBlockList][dotnet_PutBlockList] methods.</span></span>

### <a name="snapshot-billing-scenarios"></a><span data-ttu-id="968ca-185">Ögonblicksbild fakturering scenarier</span><span class="sxs-lookup"><span data-stu-id="968ca-185">Snapshot billing scenarios</span></span>
<span data-ttu-id="968ca-186">Följande scenarier visas hur debiteringar görs för en blockblobb och dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="968ca-186">The following scenarios demonstrate how charges accrue for a block blob and its snapshots.</span></span>

<span data-ttu-id="968ca-187">**Scenario 1**</span><span class="sxs-lookup"><span data-stu-id="968ca-187">**Scenario 1**</span></span>

<span data-ttu-id="968ca-188">I scenario 1 har grundläggande blob inte uppdaterats när ögonblicksbilden togs, så avgifter tas ut endast unika block 1, 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="968ca-188">In scenario 1, the base blob has not been updated after the snapshot was taken, so charges are incurred only for unique blocks 1, 2, and 3.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

<span data-ttu-id="968ca-190">**Scenario 2**</span><span class="sxs-lookup"><span data-stu-id="968ca-190">**Scenario 2**</span></span>

<span data-ttu-id="968ca-191">I scenario 2 grundläggande blob har uppdaterats, men ögonblicksbilden har inte.</span><span class="sxs-lookup"><span data-stu-id="968ca-191">In scenario 2, the base blob has been updated, but the snapshot has not.</span></span> <span data-ttu-id="968ca-192">Block 3 har uppdaterats och även om den innehåller samma data och samma ID, den är inte samma som blockerar 3 i ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="968ca-192">Block 3 was updated, and even though it contains the same data and the same ID, it is not the same as block 3 in the snapshot.</span></span> <span data-ttu-id="968ca-193">Därför debiteras kontot för fyra block.</span><span class="sxs-lookup"><span data-stu-id="968ca-193">As a result, the account is charged for four blocks.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

<span data-ttu-id="968ca-195">**Scenario 3**</span><span class="sxs-lookup"><span data-stu-id="968ca-195">**Scenario 3**</span></span>

<span data-ttu-id="968ca-196">I scenario 3 grundläggande blob har uppdaterats, men ögonblicksbilden har inte.</span><span class="sxs-lookup"><span data-stu-id="968ca-196">In scenario 3, the base blob has been updated, but the snapshot has not.</span></span> <span data-ttu-id="968ca-197">Block 3 ersattes med block 4 i basen blob, men ögonblicksbilden visar block 3.</span><span class="sxs-lookup"><span data-stu-id="968ca-197">Block 3 was replaced with block 4 in the base blob, but the snapshot still reflects block 3.</span></span> <span data-ttu-id="968ca-198">Därför debiteras kontot för fyra block.</span><span class="sxs-lookup"><span data-stu-id="968ca-198">As a result, the account is charged for four blocks.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

<span data-ttu-id="968ca-200">**Scenario 4**</span><span class="sxs-lookup"><span data-stu-id="968ca-200">**Scenario 4**</span></span>

<span data-ttu-id="968ca-201">I scenariot 4 grundläggande blob har uppdaterats helt och innehåller inga av dess ursprungliga block.</span><span class="sxs-lookup"><span data-stu-id="968ca-201">In scenario 4, the base blob has been completely updated and contains none of its original blocks.</span></span> <span data-ttu-id="968ca-202">Därför debiteras kontot för alla åtta unika block.</span><span class="sxs-lookup"><span data-stu-id="968ca-202">As a result, the account is charged for all eight unique blocks.</span></span> <span data-ttu-id="968ca-203">Den här situationen kan uppstå om du använder en metod för att uppdatera som [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], eller [UploadFromByteArray][dotnet_UploadFromByteArray], eftersom dessa metoder Ersätt hela innehållet i en blob.</span><span class="sxs-lookup"><span data-stu-id="968ca-203">This scenario can occur if you are using an update method such as [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray], because these methods replace all of the contents of a blob.</span></span>

![Azure Storage-resurser](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a><span data-ttu-id="968ca-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="968ca-205">Next steps</span></span>

* <span data-ttu-id="968ca-206">Du hittar mer information om hur du arbetar med ögonblicksbilder av virtuella datorer (VM)-disk i [säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder](storage-incremental-snapshots.md)</span><span class="sxs-lookup"><span data-stu-id="968ca-206">You can find more information about working with virtual machine (VM) disk snapshots in [Back up Azure unmanaged VM disks with incremental snapshots](storage-incremental-snapshots.md)</span></span>

* <span data-ttu-id="968ca-207">Ytterligare kodexempel med Blob storage finns [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span><span class="sxs-lookup"><span data-stu-id="968ca-207">For additional code examples using Blob storage, see [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span></span> <span data-ttu-id="968ca-208">Du kan hämta ett exempelprogram och kör den, eller bläddra koden på GitHub.</span><span class="sxs-lookup"><span data-stu-id="968ca-208">You can download a sample application and run it, or browse the code on GitHub.</span></span>

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