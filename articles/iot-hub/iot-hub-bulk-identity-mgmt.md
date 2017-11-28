---
title: "aaaImport export av identiteter för Azure IoT Hub-enhet | Microsoft Docs"
description: "Hur toouse hello Azure IoT service SDK tooperform Massredigera åtgärder mot hello identitet registret tooimport och exportera enheten identiteter. Importåtgärder aktivera toocreate, uppdatera och ta bort enheten identiteter gruppvis."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2ade1494-45ea-46a7-ade7-cf6e11ce62da
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: b67777d381e03de05d9c007b5ce6bdaf15bbb8f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a><span data-ttu-id="d3836-104">Hantera din IoT-hubb enheten identiteter i grupp</span><span class="sxs-lookup"><span data-stu-id="d3836-104">Manage your IoT Hub device identities in bulk</span></span>

<span data-ttu-id="d3836-105">Varje IoT-hubben har en identitetsregistret som du kan använda toocreate per enhet resurser i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d3836-105">Each IoT hub has an identity registry you can use toocreate per-device resources in hello service.</span></span> <span data-ttu-id="d3836-106">Hej identitetsregistret kan du också toocontrol åtkomst toohello enheten riktade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="d3836-106">hello identity registry also enables you toocontrol access toohello device-facing endpoints.</span></span> <span data-ttu-id="d3836-107">Den här artikeln beskriver hur tooimport och exportera enheten identiteter i masskopiera tooand från en identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="d3836-107">This article describes how tooimport and export device identities in bulk tooand from an identity registry.</span></span>

<span data-ttu-id="d3836-108">Import och export-åtgärder sker hello gäller *jobb* som gör att du tooexecute service massåtgärder mot en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d3836-108">Import and export operations take place in hello context of *Jobs* that enable you tooexecute bulk service operations against an IoT hub.</span></span>

<span data-ttu-id="d3836-109">Hej **RegistryManager** klassen innehåller hello **ExportDevicesAsync** och **ImportDevicesAsync** metoder som använder hello **jobbet** Framework.</span><span class="sxs-lookup"><span data-stu-id="d3836-109">hello **RegistryManager** class includes hello **ExportDevicesAsync** and **ImportDevicesAsync** methods that use hello **Job** framework.</span></span> <span data-ttu-id="d3836-110">Dessa metoder kan du tooexport, importera och synkronisera hello hela identitetsregistret en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d3836-110">These methods enable you tooexport, import, and synchronize hello entirety of an IoT hub identity registry.</span></span>

## <a name="what-are-jobs"></a><span data-ttu-id="d3836-111">Vad är jobb?</span><span class="sxs-lookup"><span data-stu-id="d3836-111">What are jobs?</span></span>

<span data-ttu-id="d3836-112">Identitet registret åtgärderna använder hello **jobbet** system när hello igen:</span><span class="sxs-lookup"><span data-stu-id="d3836-112">Identity registry operations use hello **Job** system when hello operation:</span></span>

* <span data-ttu-id="d3836-113">Har en lång körningstid jämfört med toostandard körning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d3836-113">Has a potentially long execution time compared toostandard run-time operations.</span></span>
* <span data-ttu-id="d3836-114">Returnerar en stor mängd data toohello användare.</span><span class="sxs-lookup"><span data-stu-id="d3836-114">Returns a large amount of data toohello user.</span></span>

<span data-ttu-id="d3836-115">I stället för ett enda API-anrop som väntar på eller blockerar hello resultatet av åtgärden hello hello igen asynkront skapar en **jobbet** för att IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d3836-115">Instead of a single API call waiting or blocking on hello result of hello operation, hello operation asynchronously creates a **Job** for that IoT hub.</span></span> <span data-ttu-id="d3836-116">hello igen och sedan omedelbart returnerar en **JobProperties** objekt.</span><span class="sxs-lookup"><span data-stu-id="d3836-116">hello operation then immediately returns a **JobProperties** object.</span></span>

<span data-ttu-id="d3836-117">Följande C# hello code fragment visas hur toocreate ett exportjobb:</span><span class="sxs-lookup"><span data-stu-id="d3836-117">hello following C# code snippet shows how toocreate an export job:</span></span>

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> <span data-ttu-id="d3836-118">toouse hello **RegistryManager** klassen i C#-kod, lägga till hello **Microsoft.Azure.Devices** NuGet-paketet tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d3836-118">toouse hello **RegistryManager** class in your C# code, add hello **Microsoft.Azure.Devices** NuGet package tooyour project.</span></span> <span data-ttu-id="d3836-119">Hej **RegistryManager** klassen är i hello **Microsoft.Azure.Devices** namnområde.</span><span class="sxs-lookup"><span data-stu-id="d3836-119">hello **RegistryManager** class is in hello **Microsoft.Azure.Devices** namespace.</span></span>

<span data-ttu-id="d3836-120">Du kan använda hello **RegistryManager** klassen tooquery hello tillstånd hello **jobbet** med hello returnerade **JobProperties** metadata.</span><span class="sxs-lookup"><span data-stu-id="d3836-120">You can use hello **RegistryManager** class tooquery hello state of hello **Job** using hello returned **JobProperties** metadata.</span></span>

<span data-ttu-id="d3836-121">hello visar följande kodfragment för C# hur toopoll var femte sekund toosee om hello jobbet har slutförts körs:</span><span class="sxs-lookup"><span data-stu-id="d3836-121">hello following C# code snippet shows how toopoll every five seconds toosee if hello job has finished executing:</span></span>

```csharp
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a><span data-ttu-id="d3836-122">Exportera enheter</span><span class="sxs-lookup"><span data-stu-id="d3836-122">Export devices</span></span>

<span data-ttu-id="d3836-123">Använd hello **ExportDevicesAsync** metoden tooexport hello helhet för en IoT-hubb identitet registret tooan [Azure Storage](../storage/index.md) blob-behållare med en [signatur för delad åtkomst](../storage/common/storage-security-guide.md#data-plane-security).</span><span class="sxs-lookup"><span data-stu-id="d3836-123">Use hello **ExportDevicesAsync** method tooexport hello entirety of an IoT hub identity registry tooan [Azure Storage](../storage/index.md) blob container using a [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).</span></span>

<span data-ttu-id="d3836-124">Den här metoden kan du toocreate tillförlitliga säkerhetskopior av din enhetsinformation i en blobbbehållare som du bestämmer.</span><span class="sxs-lookup"><span data-stu-id="d3836-124">This method enables you toocreate reliable backups of your device information in a blob container that you control.</span></span>

<span data-ttu-id="d3836-125">Hej **ExportDevicesAsync** kräver två parametrar:</span><span class="sxs-lookup"><span data-stu-id="d3836-125">hello **ExportDevicesAsync** method requires two parameters:</span></span>

* <span data-ttu-id="d3836-126">En *sträng* som innehåller en URI för en blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="d3836-126">A *string* that contains a URI of a blob container.</span></span> <span data-ttu-id="d3836-127">Den här URI måste innehålla en SAS-token som ger skrivbehörighet toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="d3836-127">This URI must contain a SAS token that grants write access toohello container.</span></span> <span data-ttu-id="d3836-128">hello jobbet skapar en blockblobb i den här behållaren toostore hello serialiseras exportera enhetsdata.</span><span class="sxs-lookup"><span data-stu-id="d3836-128">hello job creates a block blob in this container toostore hello serialized export device data.</span></span> <span data-ttu-id="d3836-129">hello SAS-token måste innehålla följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="d3836-129">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* <span data-ttu-id="d3836-130">En *booleskt* som anger om du vill tooexclude autentiseringsnycklar exportera data.</span><span class="sxs-lookup"><span data-stu-id="d3836-130">A *boolean* that indicates if you want tooexclude authentication keys from your export data.</span></span> <span data-ttu-id="d3836-131">Om **FALSKT**, autentiseringsnycklar ingår i utdata för export.</span><span class="sxs-lookup"><span data-stu-id="d3836-131">If **false**, authentication keys are included in export output.</span></span> <span data-ttu-id="d3836-132">I annat fall exporteras nycklar som **null**.</span><span class="sxs-lookup"><span data-stu-id="d3836-132">Otherwise, keys are exported as **null**.</span></span>

<span data-ttu-id="d3836-133">hello visar följande kodfragment för C# hur tooinitiate ett exportjobb som innehåller enheten autentiseringsnycklar i hello exportera data och sedan söka efter slutförande:</span><span class="sxs-lookup"><span data-stu-id="d3836-133">hello following C# code snippet shows how tooinitiate an export job that includes device authentication keys in hello export data and then poll for completion:</span></span>

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

<span data-ttu-id="d3836-134">hello jobbet lagrar utdata i hello tillhandahålls blob-behållaren som en blockblobb med hello namnet **devices.txt**.</span><span class="sxs-lookup"><span data-stu-id="d3836-134">hello job stores its output in hello provided blob container as a block blob with hello name **devices.txt**.</span></span> <span data-ttu-id="d3836-135">hello utdata består av JSON serialiserade data på enheten, med en enhet per rad.</span><span class="sxs-lookup"><span data-stu-id="d3836-135">hello output data consists of JSON serialized device data, with one device per line.</span></span>

<span data-ttu-id="d3836-136">hello visar följande exempel hello utdata:</span><span class="sxs-lookup"><span data-stu-id="d3836-136">hello following example shows hello output data:</span></span>

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Om en enhet har identiska data, exporteras också hello identiska data tillsammans med hello data på enheten. hello visar följande exempel det här formatet. <span data-ttu-id="d3836-139">Alla data från hello ”twinETag” rad fram hello slutet är identiska data.</span><span class="sxs-lookup"><span data-stu-id="d3836-139">All data from hello "twinETag" line until hello end are twin data.</span></span>

```json
{
   "id":"export-6d84f075-0",
   "eTag":"MQ==",
   "status":"enabled",
   "statusReason":"firstUpdate",
   "authentication":null,
   "twinETag":"AAAAAAAAAAI=",
   "tags":{
      "Location":"LivingRoom"
   },
   "properties":{
      "desired":{
         "Thermostat":{
            "Temperature":75.1,
            "Unit":"F"
         },
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
            "$lastUpdatedVersion":2,
            "Thermostat":{
               "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
               "$lastUpdatedVersion":2,
               "Temperature":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               },
               "Unit":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               }
            }
         },
         "$version":2
      },
      "reported":{
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:51.1309437Z"
         },
         "$version":1
      }
   }
}
```

<span data-ttu-id="d3836-140">Om du behöver komma åt toothis data i koden, kan du enkelt deserialisera dessa data med hello **ExportImportDevice** klass.</span><span class="sxs-lookup"><span data-stu-id="d3836-140">If you need access toothis data in code, you can easily deserialize this data using hello **ExportImportDevice** class.</span></span> <span data-ttu-id="d3836-141">hello visar följande kodfragment för C# hur tooread enhetsinformation som var tidigare exporterade tooa blockblob:</span><span class="sxs-lookup"><span data-stu-id="d3836-141">hello following C# code snippet shows how tooread device information that was previously exported tooa block blob:</span></span>

```csharp
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), null, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [!NOTE]
> <span data-ttu-id="d3836-142">Du kan också använda hello **GetDevicesAsync** metod för hello **RegistryManager** klassen toofetch en lista över dina enheter.</span><span class="sxs-lookup"><span data-stu-id="d3836-142">You can also use hello **GetDevicesAsync** method of hello **RegistryManager** class toofetch a list of your devices.</span></span> <span data-ttu-id="d3836-143">Den här metoden har dock en hård fjärrskrivbordsanslutning 1000 hello antalet enhetsobjekt som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="d3836-143">However, this approach has a hard cap of 1000 on hello number of device objects that are returned.</span></span> <span data-ttu-id="d3836-144">hello förväntades användningsfall för hello **GetDevicesAsync** metoden är för utveckling scenarier tooaid felsökning och rekommenderas inte för produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="d3836-144">hello expected use case for hello **GetDevicesAsync** method is for development scenarios tooaid debugging and is not recommended for production workloads.</span></span>

## <a name="import-devices"></a><span data-ttu-id="d3836-145">Importera enheter</span><span class="sxs-lookup"><span data-stu-id="d3836-145">Import devices</span></span>

<span data-ttu-id="d3836-146">Hej **ImportDevicesAsync** metod i hello **RegistryManager** klassen kan du tooperform import och synkronisering massåtgärder i en IoT-hubb identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="d3836-146">hello **ImportDevicesAsync** method in hello **RegistryManager** class enables you tooperform bulk import and synchronization operations in an IoT hub identity registry.</span></span> <span data-ttu-id="d3836-147">Som hello **ExportDevicesAsync** metod, hello **ImportDevicesAsync** metoden använder hello **jobbet** framework.</span><span class="sxs-lookup"><span data-stu-id="d3836-147">Like hello **ExportDevicesAsync** method, hello **ImportDevicesAsync** method uses hello **Job** framework.</span></span>

<span data-ttu-id="d3836-148">Var noga med hello **ImportDevicesAsync** metoden eftersom i tillägg tooprovisioning nya enheter i din identitetsregistret, kan den också uppdatera och ta bort befintliga enheter.</span><span class="sxs-lookup"><span data-stu-id="d3836-148">Take care using hello **ImportDevicesAsync** method because in addition tooprovisioning new devices in your identity registry, it can also update and delete existing devices.</span></span>

> [!WARNING]
> <span data-ttu-id="d3836-149">En importen kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="d3836-149">An import operation cannot be undone.</span></span> <span data-ttu-id="d3836-150">Säkerhetskopiera dina befintliga data med hjälp av hello alltid **ExportDevicesAsync** metoden tooanother blob-behållare innan du gör bulk ändras tooyour identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="d3836-150">Always back up your existing data using hello **ExportDevicesAsync** method tooanother blob container before you make bulk changes tooyour identity registry.</span></span>

<span data-ttu-id="d3836-151">Hej **ImportDevicesAsync** metoden har två parametrar:</span><span class="sxs-lookup"><span data-stu-id="d3836-151">hello **ImportDevicesAsync** method takes two parameters:</span></span>

* <span data-ttu-id="d3836-152">En *sträng* som innehåller en URI för en [Azure Storage](../storage/index.md) blob-behållaren toouse som *inkommande* toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="d3836-152">A *string* that contains a URI of an [Azure Storage](../storage/index.md) blob container toouse as *input* toohello job.</span></span> <span data-ttu-id="d3836-153">Den här URI måste innehålla en SAS-token som ger läsbehörighet toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="d3836-153">This URI must contain a SAS token that grants read access toohello container.</span></span> <span data-ttu-id="d3836-154">Den här behållaren måste innehålla en blob med hello namnet **devices.txt** som innehåller hello serialiseras enheten data tooimport till din identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="d3836-154">This container must contain a blob with hello name **devices.txt** that contains hello serialized device data tooimport into your identity registry.</span></span> <span data-ttu-id="d3836-155">hello importera data måste innehålla enhetsinformation i hello samma JSON-format som hello **ExportImportDevice** jobbet används när den skapar en **devices.txt** blob.</span><span class="sxs-lookup"><span data-stu-id="d3836-155">hello import data must contain device information in hello same JSON format that hello **ExportImportDevice** job uses when it creates a **devices.txt** blob.</span></span> <span data-ttu-id="d3836-156">hello SAS-token måste innehålla följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="d3836-156">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* <span data-ttu-id="d3836-157">En *sträng* som innehåller en URI för en [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob-behållaren toouse som *utdata* hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="d3836-157">A *string* that contains a URI of an [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob container toouse as *output* from hello job.</span></span> <span data-ttu-id="d3836-158">hello jobbet skapar en blockblobb i den här behållaren toostore den felinformation från hello slutförd import **jobbet**.</span><span class="sxs-lookup"><span data-stu-id="d3836-158">hello job creates a block blob in this container toostore any error information from hello completed import **Job**.</span></span> <span data-ttu-id="d3836-159">hello SAS-token måste innehålla följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="d3836-159">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> <span data-ttu-id="d3836-160">hello två parametrar kan peka toohello samma blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="d3836-160">hello two parameters can point toohello same blob container.</span></span> <span data-ttu-id="d3836-161">hello separat parametrar kan bara mer kontroll över dina data eftersom hello utdata behållare kräver ytterligare behörighet.</span><span class="sxs-lookup"><span data-stu-id="d3836-161">hello separate parameters simply enable more control over your data as hello output container requires additional permissions.</span></span>

<span data-ttu-id="d3836-162">Följande C# hello code fragment visas hur tooinitiate importen:</span><span class="sxs-lookup"><span data-stu-id="d3836-162">hello following C# code snippet shows how tooinitiate an import job:</span></span>

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

<span data-ttu-id="d3836-163">Den här metoden kan också vara används tooimport hello data för hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="d3836-163">This method can also be used tooimport hello data for hello device twin.</span></span> <span data-ttu-id="d3836-164">hello format för hello-indata är hello samma som hello-format som visas i hello **ExportDevicesAsync** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d3836-164">hello format for hello data input is hello same as hello format shown in hello **ExportDevicesAsync** section.</span></span> <span data-ttu-id="d3836-165">På så sätt kan importera du hello exporterade data.</span><span class="sxs-lookup"><span data-stu-id="d3836-165">In this way, you can reimport hello exported data.</span></span> <span data-ttu-id="d3836-166">Hej **$metadata** är valfritt.</span><span class="sxs-lookup"><span data-stu-id="d3836-166">hello **$metadata** is optional.</span></span>

## <a name="import-behavior"></a><span data-ttu-id="d3836-167">Importera beteende</span><span class="sxs-lookup"><span data-stu-id="d3836-167">Import behavior</span></span>

<span data-ttu-id="d3836-168">Du kan använda hello **ImportDevicesAsync** metoden tooperform hello följande Massredigera åtgärder i identitetsregistret:</span><span class="sxs-lookup"><span data-stu-id="d3836-168">You can use hello **ImportDevicesAsync** method tooperform hello following bulk operations in your identity registry:</span></span>

* <span data-ttu-id="d3836-169">Massinläsning registrering av nya enheter</span><span class="sxs-lookup"><span data-stu-id="d3836-169">Bulk registration of new devices</span></span>
* <span data-ttu-id="d3836-170">Massinläsning borttagningar av befintliga enheter</span><span class="sxs-lookup"><span data-stu-id="d3836-170">Bulk deletions of existing devices</span></span>
* <span data-ttu-id="d3836-171">Massredigera statusändringar (aktivera eller inaktivera enheter)</span><span class="sxs-lookup"><span data-stu-id="d3836-171">Bulk status changes (enable or disable devices)</span></span>
* <span data-ttu-id="d3836-172">Massinläsning tilldelning av nya nycklar för autentisering av enheten</span><span class="sxs-lookup"><span data-stu-id="d3836-172">Bulk assignment of new device authentication keys</span></span>
* <span data-ttu-id="d3836-173">Massinläsning auto-omskapande av enheten autentiseringsnycklar</span><span class="sxs-lookup"><span data-stu-id="d3836-173">Bulk auto-regeneration of device authentication keys</span></span>
* <span data-ttu-id="d3836-174">Massuppdatering av identiska data</span><span class="sxs-lookup"><span data-stu-id="d3836-174">Bulk update of twin data</span></span>

<span data-ttu-id="d3836-175">Du kan utföra en kombination av hello föregående åtgärder i en enda **ImportDevicesAsync** anropa.</span><span class="sxs-lookup"><span data-stu-id="d3836-175">You can perform any combination of hello preceding operations within a single **ImportDevicesAsync** call.</span></span> <span data-ttu-id="d3836-176">Exempelvis kan du registrera nya enheter och ta bort eller uppdatera befintliga enheter på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d3836-176">For example, you can register new devices and delete or update existing devices at hello same time.</span></span> <span data-ttu-id="d3836-177">När den används tillsammans med hello **ExportDevicesAsync** metod du helt kan migrera alla dina enheter från en IoT-hubb tooanother.</span><span class="sxs-lookup"><span data-stu-id="d3836-177">When used along with hello **ExportDevicesAsync** method, you can completely migrate all your devices from one IoT hub tooanother.</span></span>

<span data-ttu-id="d3836-178">Om hello importfilen innehåller dubbla metadata, skrivs dessa metadata hello befintliga dubbla metadata.</span><span class="sxs-lookup"><span data-stu-id="d3836-178">If hello import file includes twin metadata, then this metadata overwrites hello existing twin metadata.</span></span> <span data-ttu-id="d3836-179">Om hello importfilen inte innehåller dubbla metadata sedan endast hello `lastUpdateTime` metadata uppdateras med hjälp av hello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="d3836-179">If hello import file does not include twin metadata, then only hello `lastUpdateTime` metadata is updated using hello current time.</span></span>

<span data-ttu-id="d3836-180">Använd hello valfria **ImportMode %** egenskap i hello importera serialiseringsdata för varje enhet toocontrol hello importera processen per enhet.</span><span class="sxs-lookup"><span data-stu-id="d3836-180">Use hello optional **importMode** property in hello import serialization data for each device toocontrol hello import process per-device.</span></span> <span data-ttu-id="d3836-181">Hej **ImportMode %** -egenskap har hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="d3836-181">hello **importMode** property has hello following options:</span></span>

| <span data-ttu-id="d3836-182">ImportMode %</span><span class="sxs-lookup"><span data-stu-id="d3836-182">importMode</span></span> | <span data-ttu-id="d3836-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d3836-183">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d3836-184">**createOrUpdate**</span><span class="sxs-lookup"><span data-stu-id="d3836-184">**createOrUpdate**</span></span> |<span data-ttu-id="d3836-185">Om en enhet inte finns med hello anges **id**, den nyligen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="d3836-185">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="d3836-186">Om hello enheten redan befintlig information skrivs över med hello angivna indata utan hänsyn toohello **ETag** värde.</span><span class="sxs-lookup"><span data-stu-id="d3836-186">If hello device already exists, existing information is overwritten with hello provided input data without regard toohello **ETag** value.</span></span> <br> <span data-ttu-id="d3836-187">hello användare kan du ange identiska data tillsammans med hello data på enheten.</span><span class="sxs-lookup"><span data-stu-id="d3836-187">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="d3836-188">hello dubbla etag bearbetas om anges separat från hello enhetens etag.</span><span class="sxs-lookup"><span data-stu-id="d3836-188">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="d3836-189">Om det finns en felaktig matchning med hello befintliga dubbla etag, skrivs ett fel toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-189">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="d3836-190">**skapa**</span><span class="sxs-lookup"><span data-stu-id="d3836-190">**create**</span></span> |<span data-ttu-id="d3836-191">Om en enhet inte finns med hello anges **id**, den nyligen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="d3836-191">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="d3836-192">Om hello enheten redan finns skrivs ett fel toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-192">If hello device already exists, an error is written toohello log file.</span></span> <br> <span data-ttu-id="d3836-193">hello användare kan du ange identiska data tillsammans med hello data på enheten.</span><span class="sxs-lookup"><span data-stu-id="d3836-193">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="d3836-194">hello dubbla etag bearbetas om anges separat från hello enhetens etag.</span><span class="sxs-lookup"><span data-stu-id="d3836-194">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="d3836-195">Om det finns en felaktig matchning med hello befintliga dubbla etag, skrivs ett fel toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-195">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="d3836-196">**Uppdatera**</span><span class="sxs-lookup"><span data-stu-id="d3836-196">**update**</span></span> |<span data-ttu-id="d3836-197">Om det finns redan en enhet med hello anges **id**, befintlig information skrivs över med hello angivna indata utan hänsyn toohello **ETag** värde.</span><span class="sxs-lookup"><span data-stu-id="d3836-197">If a device already exists with hello specified **id**, existing information is overwritten with hello provided input data without regard toohello **ETag** value.</span></span> <br/><span data-ttu-id="d3836-198">Om hello enheten inte finns, är ett fel skrivs toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-198">If hello device does not exist, an error is written toohello log file.</span></span> |
| <span data-ttu-id="d3836-199">**updateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="d3836-199">**updateIfMatchETag**</span></span> |<span data-ttu-id="d3836-200">Om det finns redan en enhet med hello anges **id**, befintlig information skrivs över med hello angivna indata, om det finns en **ETag** matchar.</span><span class="sxs-lookup"><span data-stu-id="d3836-200">If a device already exists with hello specified **id**, existing information is overwritten with hello provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="d3836-201">Om hello enheten inte finns, är ett fel skrivs toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-201">If hello device does not exist, an error is written toohello log file.</span></span> <br/><span data-ttu-id="d3836-202">Om det finns en **ETag** matchningsfel, ett fel skrivs toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-202">If there is an **ETag** mismatch, an error is written toohello log file.</span></span> |
| <span data-ttu-id="d3836-203">**createOrUpdateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="d3836-203">**createOrUpdateIfMatchETag**</span></span> |<span data-ttu-id="d3836-204">Om en enhet inte finns med hello anges **id**, den nyligen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="d3836-204">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="d3836-205">Om hello enheten redan befintlig information skrivs över med hello angivna indata, om det finns en **ETag** matchar.</span><span class="sxs-lookup"><span data-stu-id="d3836-205">If hello device already exists, existing information is overwritten with hello provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="d3836-206">Om det finns en **ETag** matchningsfel, ett fel skrivs toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-206">If there is an **ETag** mismatch, an error is written toohello log file.</span></span> <br> <span data-ttu-id="d3836-207">hello användare kan du ange identiska data tillsammans med hello data på enheten.</span><span class="sxs-lookup"><span data-stu-id="d3836-207">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="d3836-208">hello dubbla etag bearbetas om anges separat från hello enhetens etag.</span><span class="sxs-lookup"><span data-stu-id="d3836-208">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="d3836-209">Om det finns en felaktig matchning med hello befintliga dubbla etag, skrivs ett fel toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-209">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="d3836-210">**ta bort**</span><span class="sxs-lookup"><span data-stu-id="d3836-210">**delete**</span></span> |<span data-ttu-id="d3836-211">Om det finns redan en enhet med hello anges **id**, det har tagits bort utan hänsyn toohello **ETag** värde.</span><span class="sxs-lookup"><span data-stu-id="d3836-211">If a device already exists with hello specified **id**, it is deleted without regard toohello **ETag** value.</span></span> <br/><span data-ttu-id="d3836-212">Om hello enheten inte finns, är ett fel skrivs toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-212">If hello device does not exist, an error is written toohello log file.</span></span> |
| <span data-ttu-id="d3836-213">**deleteIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="d3836-213">**deleteIfMatchETag**</span></span> |<span data-ttu-id="d3836-214">Om det finns redan en enhet med hello anges **id**, raderas om det finns en **ETag** matchar.</span><span class="sxs-lookup"><span data-stu-id="d3836-214">If a device already exists with hello specified **id**, it is deleted only if there is an **ETag** match.</span></span> <span data-ttu-id="d3836-215">Om hello enheten inte finns, är ett fel skrivs toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-215">If hello device does not exist, an error is written toohello log file.</span></span> <br/><span data-ttu-id="d3836-216">Om det finns en ETag-Typfel, skrivs ett fel toohello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="d3836-216">If there is an ETag mismatch, an error is written toohello log file.</span></span> |

> [!NOTE]
> <span data-ttu-id="d3836-217">Om hello serialiseringsdata inte explicit definiera en **ImportMode %** flaggan för en enhet, används som standard för**createOrUpdate** under hello importåtgärden.</span><span class="sxs-lookup"><span data-stu-id="d3836-217">If hello serialization data does not explicitly define an **importMode** flag for a device, it defaults too**createOrUpdate** during hello import operation.</span></span>

## <a name="import-devices-example--bulk-device-provisioning"></a><span data-ttu-id="d3836-218">Importera enheter exempel – masskopiera enhetsetableringen</span><span class="sxs-lookup"><span data-stu-id="d3836-218">Import devices example – bulk device provisioning</span></span>

<span data-ttu-id="d3836-219">hello följande C# kodexempel illustrerar hur toogenerate flera identiteter i enheten som:</span><span class="sxs-lookup"><span data-stu-id="d3836-219">hello following C# code sample illustrates how toogenerate multiple device identities that:</span></span>

* <span data-ttu-id="d3836-220">Inkludera autentiseringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="d3836-220">Include authentication keys.</span></span>
* <span data-ttu-id="d3836-221">Skriva den enheten information tooa blockblob.</span><span class="sxs-lookup"><span data-stu-id="d3836-221">Write that device information tooa block blob.</span></span>
* <span data-ttu-id="d3836-222">Importera hello enheter till hello identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="d3836-222">Import hello devices into hello identity registry.</span></span>

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in hello Microsoft.Azure.Devices.Common namespace
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device toohello list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write hello list toohello blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using hello blob tooadd new devices
// Log information related toohello job is written toohello same container
// This normally takes 1 minute per 100 devices
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a><span data-ttu-id="d3836-223">Importera enheter exempel – massborttagning</span><span class="sxs-lookup"><span data-stu-id="d3836-223">Import devices example – bulk deletion</span></span>

<span data-ttu-id="d3836-224">hello visar följande kodexempel hur toodelete hello enheter som du lade till i hello kodexemplet:</span><span class="sxs-lookup"><span data-stu-id="d3836-224">hello following code sample shows you how toodelete hello devices you added using hello previous code sample:</span></span>

```csharp
// Step 1: Update each device's ImportMode toobe Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back tooan ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write hello new import data back toohello block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using hello same blob toodelete all devices
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="get-hello-container-sas-uri"></a><span data-ttu-id="d3836-225">Hämta hello behållarens SAS-URI</span><span class="sxs-lookup"><span data-stu-id="d3836-225">Get hello container SAS URI</span></span>

<span data-ttu-id="d3836-226">hello följande kodexempel visar hur toogenerate en [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) med läsa, skriva och ta bort behörigheter för en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="d3836-226">hello following code sample shows you how toogenerate a [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) with read, write, and delete permissions for a blob container:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set hello expiry time and permissions for hello container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate hello shared access signature on hello container,
  // setting hello constraints directly on hello signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return hello URI string for hello container,
  // including hello SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a><span data-ttu-id="d3836-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3836-227">Next steps</span></span>

<span data-ttu-id="d3836-228">I den här artikeln har du lärt dig hur tooperform Massredigera åtgärder mot hello identitetsregistret i en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d3836-228">In this article, you learned how tooperform bulk operations against hello identity registry in an IoT hub.</span></span> <span data-ttu-id="d3836-229">Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="d3836-229">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="d3836-230">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="d3836-230">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="d3836-231">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="d3836-231">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="d3836-232">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="d3836-232">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d3836-233">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="d3836-233">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="d3836-234">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d3836-234">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
