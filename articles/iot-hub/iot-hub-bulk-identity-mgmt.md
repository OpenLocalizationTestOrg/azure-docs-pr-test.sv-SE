---
title: "Importera export av identiteter för Azure IoT Hub-enhet | Microsoft Docs"
description: "Hur du använder tjänsten Azure IoT SDK för att utföra massåtgärder mot identitetsregistret att importera och exportera enheten identiteter. Importera åtgärder kan du skapa, uppdatera och ta bort enheten identiteter gruppvis."
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
ms.openlocfilehash: ad2c6d585eef5450f7f0912ffa4753fe80d86b37
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a><span data-ttu-id="2b05c-104">Hantera din IoT-hubb enheten identiteter i grupp</span><span class="sxs-lookup"><span data-stu-id="2b05c-104">Manage your IoT Hub device identities in bulk</span></span>

<span data-ttu-id="2b05c-105">Varje IoT-hubben har en identitetsregistret som du kan använda för att skapa per enhet resurser i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2b05c-105">Each IoT hub has an identity registry you can use to create per-device resources in the service.</span></span> <span data-ttu-id="2b05c-106">Identitetsregistret kan du styra åtkomsten till enheter riktade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="2b05c-106">The identity registry also enables you to control access to the device-facing endpoints.</span></span> <span data-ttu-id="2b05c-107">Den här artikeln beskriver hur du importerar och exporterar enheten identiteter gruppvis till och från en identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="2b05c-107">This article describes how to import and export device identities in bulk to and from an identity registry.</span></span>

<span data-ttu-id="2b05c-108">Import och export-åtgärder sker i samband med *jobb* som gör att du kan köra tjänsten massåtgärder mot en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2b05c-108">Import and export operations take place in the context of *Jobs* that enable you to execute bulk service operations against an IoT hub.</span></span>

<span data-ttu-id="2b05c-109">Den **RegistryManager** klassen innehåller den **ExportDevicesAsync** och **ImportDevicesAsync** metoder som använder den **jobbet** framework.</span><span class="sxs-lookup"><span data-stu-id="2b05c-109">The **RegistryManager** class includes the **ExportDevicesAsync** and **ImportDevicesAsync** methods that use the **Job** framework.</span></span> <span data-ttu-id="2b05c-110">Dessa metoder kan du exportera, importera och synkronisera identitetsregistret en IoT-hubb i sin helhet.</span><span class="sxs-lookup"><span data-stu-id="2b05c-110">These methods enable you to export, import, and synchronize the entirety of an IoT hub identity registry.</span></span>

## <a name="what-are-jobs"></a><span data-ttu-id="2b05c-111">Vad är jobb?</span><span class="sxs-lookup"><span data-stu-id="2b05c-111">What are jobs?</span></span>

<span data-ttu-id="2b05c-112">Identitet registret åtgärderna använder den **jobbet** system när åtgärden:</span><span class="sxs-lookup"><span data-stu-id="2b05c-112">Identity registry operations use the **Job** system when the operation:</span></span>

* <span data-ttu-id="2b05c-113">Har en lång körningstid jämfört med standardåtgärder för körning.</span><span class="sxs-lookup"><span data-stu-id="2b05c-113">Has a potentially long execution time compared to standard run-time operations.</span></span>
* <span data-ttu-id="2b05c-114">Returnerar en stor mängd data för användaren.</span><span class="sxs-lookup"><span data-stu-id="2b05c-114">Returns a large amount of data to the user.</span></span>

<span data-ttu-id="2b05c-115">I stället för ett enda API-anrop som väntar på eller blockerar på resultatet av åtgärden igen asynkront skapar en **jobbet** för att IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2b05c-115">Instead of a single API call waiting or blocking on the result of the operation, the operation asynchronously creates a **Job** for that IoT hub.</span></span> <span data-ttu-id="2b05c-116">Åtgärden sedan omedelbart returnerar en **JobProperties** objekt.</span><span class="sxs-lookup"><span data-stu-id="2b05c-116">The operation then immediately returns a **JobProperties** object.</span></span>

<span data-ttu-id="2b05c-117">Följande C# kodfragment visar hur du skapar ett exportjobb:</span><span class="sxs-lookup"><span data-stu-id="2b05c-117">The following C# code snippet shows how to create an export job:</span></span>

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> <span data-ttu-id="2b05c-118">Att använda den **RegistryManager** klassen i C#-kod, lägga till den **Microsoft.Azure.Devices** NuGet-paket i projektet.</span><span class="sxs-lookup"><span data-stu-id="2b05c-118">To use the **RegistryManager** class in your C# code, add the **Microsoft.Azure.Devices** NuGet package to your project.</span></span> <span data-ttu-id="2b05c-119">Den **RegistryManager** klassen är i den **Microsoft.Azure.Devices** namnområde.</span><span class="sxs-lookup"><span data-stu-id="2b05c-119">The **RegistryManager** class is in the **Microsoft.Azure.Devices** namespace.</span></span>

<span data-ttu-id="2b05c-120">Du kan använda den **RegistryManager** klassen för att fråga efter tillståndet för den **jobbet** med hjälp av den returnerade **JobProperties** metadata.</span><span class="sxs-lookup"><span data-stu-id="2b05c-120">You can use the **RegistryManager** class to query the state of the **Job** using the returned **JobProperties** metadata.</span></span>

<span data-ttu-id="2b05c-121">Följande C# kodfragmentet visas hur du avsöker var femte sekund för att se om jobbet har slutförts körs:</span><span class="sxs-lookup"><span data-stu-id="2b05c-121">The following C# code snippet shows how to poll every five seconds to see if the job has finished executing:</span></span>

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

## <a name="export-devices"></a><span data-ttu-id="2b05c-122">Exportera enheter</span><span class="sxs-lookup"><span data-stu-id="2b05c-122">Export devices</span></span>

<span data-ttu-id="2b05c-123">Använd den **ExportDevicesAsync** metod för att exportera i sin helhet en IoT-hubb identitetsregistret till en [Azure Storage](../storage/index.md) blob-behållare med en [signatur för delad åtkomst](../storage/common/storage-security-guide.md#data-plane-security).</span><span class="sxs-lookup"><span data-stu-id="2b05c-123">Use the **ExportDevicesAsync** method to export the entirety of an IoT hub identity registry to an [Azure Storage](../storage/index.md) blob container using a [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).</span></span>

<span data-ttu-id="2b05c-124">Den här metoden kan du skapa pålitliga säkerhetskopieringar av din enhetsinformation i en blobbbehållare som du bestämmer.</span><span class="sxs-lookup"><span data-stu-id="2b05c-124">This method enables you to create reliable backups of your device information in a blob container that you control.</span></span>

<span data-ttu-id="2b05c-125">Den **ExportDevicesAsync** kräver två parametrar:</span><span class="sxs-lookup"><span data-stu-id="2b05c-125">The **ExportDevicesAsync** method requires two parameters:</span></span>

* <span data-ttu-id="2b05c-126">En *sträng* som innehåller en URI för en blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="2b05c-126">A *string* that contains a URI of a blob container.</span></span> <span data-ttu-id="2b05c-127">Den här URI måste innehålla en SAS-token som ger åtkomst till behållaren.</span><span class="sxs-lookup"><span data-stu-id="2b05c-127">This URI must contain a SAS token that grants write access to the container.</span></span> <span data-ttu-id="2b05c-128">Jobbet skapar en blockblobb i behållaren för lagring av enhetsdata serialiserade export.</span><span class="sxs-lookup"><span data-stu-id="2b05c-128">The job creates a block blob in this container to store the serialized export device data.</span></span> <span data-ttu-id="2b05c-129">SAS-token måste innehålla följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="2b05c-129">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* <span data-ttu-id="2b05c-130">En *booleskt* som anger om du vill undanta autentiseringsnycklar från exportera data.</span><span class="sxs-lookup"><span data-stu-id="2b05c-130">A *boolean* that indicates if you want to exclude authentication keys from your export data.</span></span> <span data-ttu-id="2b05c-131">Om **FALSKT**, autentiseringsnycklar ingår i utdata för export.</span><span class="sxs-lookup"><span data-stu-id="2b05c-131">If **false**, authentication keys are included in export output.</span></span> <span data-ttu-id="2b05c-132">I annat fall exporteras nycklar som **null**.</span><span class="sxs-lookup"><span data-stu-id="2b05c-132">Otherwise, keys are exported as **null**.</span></span>

<span data-ttu-id="2b05c-133">Följande C# kodfragment visar hur du initierar ett exportjobb som innehåller enheten autentiseringsnycklar i exporterade data och sedan söka efter slutförande:</span><span class="sxs-lookup"><span data-stu-id="2b05c-133">The following C# code snippet shows how to initiate an export job that includes device authentication keys in the export data and then poll for completion:</span></span>

```csharp
// Call an export job on the IoT Hub to retrieve all devices
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

<span data-ttu-id="2b05c-134">Jobbet lagrar utdata i angivna blob-behållaren som en blockblobb med namnet **devices.txt**.</span><span class="sxs-lookup"><span data-stu-id="2b05c-134">The job stores its output in the provided blob container as a block blob with the name **devices.txt**.</span></span> <span data-ttu-id="2b05c-135">Utdata består av JSON serialiserade data på enheten, med en enhet per rad.</span><span class="sxs-lookup"><span data-stu-id="2b05c-135">The output data consists of JSON serialized device data, with one device per line.</span></span>

<span data-ttu-id="2b05c-136">I följande exempel visar utdata:</span><span class="sxs-lookup"><span data-stu-id="2b05c-136">The following example shows the output data:</span></span>

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Om en enhet har identiska data, exporteras också identiska data tillsammans med enhetsdata. I följande exempel visas det här formatet. <span data-ttu-id="2b05c-139">Alla data från raden ”twinETag” ända tills den är identiska data.</span><span class="sxs-lookup"><span data-stu-id="2b05c-139">All data from the "twinETag" line until the end are twin data.</span></span>

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

<span data-ttu-id="2b05c-140">Om du behöver åtkomst till dessa data i kod kan du enkelt deserialisera data med den **ExportImportDevice** klass.</span><span class="sxs-lookup"><span data-stu-id="2b05c-140">If you need access to this data in code, you can easily deserialize this data using the **ExportImportDevice** class.</span></span> <span data-ttu-id="2b05c-141">Följande C# kodfragment visar hur de ska läsa enhetsinformation som tidigare har exporterats till en blockblobb:</span><span class="sxs-lookup"><span data-stu-id="2b05c-141">The following C# code snippet shows how to read device information that was previously exported to a block blob:</span></span>

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
> <span data-ttu-id="2b05c-142">Du kan också använda den **GetDevicesAsync** metod för den **RegistryManager** klassen för att hämta en lista över dina enheter.</span><span class="sxs-lookup"><span data-stu-id="2b05c-142">You can also use the **GetDevicesAsync** method of the **RegistryManager** class to fetch a list of your devices.</span></span> <span data-ttu-id="2b05c-143">Den här metoden har dock en hård fjärrskrivbordsanslutning 1000 på antalet enhetsobjekt som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="2b05c-143">However, this approach has a hard cap of 1000 on the number of device objects that are returned.</span></span> <span data-ttu-id="2b05c-144">Det förväntade användningsfallet för den **GetDevicesAsync** metoden är för utvecklingsscenarier att underlätta felsökning och rekommenderas inte för produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="2b05c-144">The expected use case for the **GetDevicesAsync** method is for development scenarios to aid debugging and is not recommended for production workloads.</span></span>

## <a name="import-devices"></a><span data-ttu-id="2b05c-145">Importera enheter</span><span class="sxs-lookup"><span data-stu-id="2b05c-145">Import devices</span></span>

<span data-ttu-id="2b05c-146">Den **ImportDevicesAsync** metod i den **RegistryManager** klassen kan du utföra massåtgärder för import och synkronisering i en IoT-hubb identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="2b05c-146">The **ImportDevicesAsync** method in the **RegistryManager** class enables you to perform bulk import and synchronization operations in an IoT hub identity registry.</span></span> <span data-ttu-id="2b05c-147">Som det **ExportDevicesAsync** -metoden i **ImportDevicesAsync** metoden använder den **jobbet** framework.</span><span class="sxs-lookup"><span data-stu-id="2b05c-147">Like the **ExportDevicesAsync** method, the **ImportDevicesAsync** method uses the **Job** framework.</span></span>

<span data-ttu-id="2b05c-148">Var noga med hjälp av den **ImportDevicesAsync** metoden eftersom förutom att etablera nya enheter i din identitetsregistret den även uppdatera och ta bort befintliga enheter.</span><span class="sxs-lookup"><span data-stu-id="2b05c-148">Take care using the **ImportDevicesAsync** method because in addition to provisioning new devices in your identity registry, it can also update and delete existing devices.</span></span>

> [!WARNING]
> <span data-ttu-id="2b05c-149">En importen kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="2b05c-149">An import operation cannot be undone.</span></span> <span data-ttu-id="2b05c-150">Säkerhetskopiera alltid dina befintliga data med hjälp av den **ExportDevicesAsync** metod till en annan blob-behållare innan du gör bulk ändringar i identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="2b05c-150">Always back up your existing data using the **ExportDevicesAsync** method to another blob container before you make bulk changes to your identity registry.</span></span>

<span data-ttu-id="2b05c-151">Den **ImportDevicesAsync** metoden har två parametrar:</span><span class="sxs-lookup"><span data-stu-id="2b05c-151">The **ImportDevicesAsync** method takes two parameters:</span></span>

* <span data-ttu-id="2b05c-152">En *sträng* som innehåller en URI för en [Azure Storage](../storage/index.md) blob-behållaren ska användas som *inkommande* i jobbet.</span><span class="sxs-lookup"><span data-stu-id="2b05c-152">A *string* that contains a URI of an [Azure Storage](../storage/index.md) blob container to use as *input* to the job.</span></span> <span data-ttu-id="2b05c-153">Den här URI måste innehålla en SAS-token som ger läsbehörighet till behållaren.</span><span class="sxs-lookup"><span data-stu-id="2b05c-153">This URI must contain a SAS token that grants read access to the container.</span></span> <span data-ttu-id="2b05c-154">Den här behållaren måste innehålla en blob med namnet **devices.txt** som innehåller serialiserade enhetsdata importera till din identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="2b05c-154">This container must contain a blob with the name **devices.txt** that contains the serialized device data to import into your identity registry.</span></span> <span data-ttu-id="2b05c-155">Importera data måste innehålla enhetsinformation i samma JSON-format som den **ExportImportDevice** jobbet används när den skapar en **devices.txt** blob.</span><span class="sxs-lookup"><span data-stu-id="2b05c-155">The import data must contain device information in the same JSON format that the **ExportImportDevice** job uses when it creates a **devices.txt** blob.</span></span> <span data-ttu-id="2b05c-156">SAS-token måste innehålla följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="2b05c-156">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* <span data-ttu-id="2b05c-157">En *sträng* som innehåller en URI för en [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob-behållaren ska användas som *utdata* från jobbet.</span><span class="sxs-lookup"><span data-stu-id="2b05c-157">A *string* that contains a URI of an [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob container to use as *output* from the job.</span></span> <span data-ttu-id="2b05c-158">Jobbet skapar en blockblobb i behållaren att spara felinformation om från den slutförda importen **jobbet**.</span><span class="sxs-lookup"><span data-stu-id="2b05c-158">The job creates a block blob in this container to store any error information from the completed import **Job**.</span></span> <span data-ttu-id="2b05c-159">SAS-token måste innehålla följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="2b05c-159">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> <span data-ttu-id="2b05c-160">De två parametrarna peka på samma blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="2b05c-160">The two parameters can point to the same blob container.</span></span> <span data-ttu-id="2b05c-161">Separata parametrar kan bara mer kontroll över dina data som kräver ytterligare behörigheter för behållaren utdata.</span><span class="sxs-lookup"><span data-stu-id="2b05c-161">The separate parameters simply enable more control over your data as the output container requires additional permissions.</span></span>

<span data-ttu-id="2b05c-162">Följande C# kodfragmentet visas hur du initierar ett importjobb:</span><span class="sxs-lookup"><span data-stu-id="2b05c-162">The following C# code snippet shows how to initiate an import job:</span></span>

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

<span data-ttu-id="2b05c-163">Den här metoden kan även användas för import av data för dubbla för enheten.</span><span class="sxs-lookup"><span data-stu-id="2b05c-163">This method can also be used to import the data for the device twin.</span></span> <span data-ttu-id="2b05c-164">Formatet för indata är detsamma som visas i den **ExportDevicesAsync** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2b05c-164">The format for the data input is the same as the format shown in the **ExportDevicesAsync** section.</span></span> <span data-ttu-id="2b05c-165">På så sätt kan importera du den exporterade data.</span><span class="sxs-lookup"><span data-stu-id="2b05c-165">In this way, you can reimport the exported data.</span></span> <span data-ttu-id="2b05c-166">Den **$metadata** är valfritt.</span><span class="sxs-lookup"><span data-stu-id="2b05c-166">The **$metadata** is optional.</span></span>

## <a name="import-behavior"></a><span data-ttu-id="2b05c-167">Importera beteende</span><span class="sxs-lookup"><span data-stu-id="2b05c-167">Import behavior</span></span>

<span data-ttu-id="2b05c-168">Du kan använda den **ImportDevicesAsync** metod för att utföra följande massåtgärder i identitetsregistret:</span><span class="sxs-lookup"><span data-stu-id="2b05c-168">You can use the **ImportDevicesAsync** method to perform the following bulk operations in your identity registry:</span></span>

* <span data-ttu-id="2b05c-169">Massinläsning registrering av nya enheter</span><span class="sxs-lookup"><span data-stu-id="2b05c-169">Bulk registration of new devices</span></span>
* <span data-ttu-id="2b05c-170">Massinläsning borttagningar av befintliga enheter</span><span class="sxs-lookup"><span data-stu-id="2b05c-170">Bulk deletions of existing devices</span></span>
* <span data-ttu-id="2b05c-171">Massredigera statusändringar (aktivera eller inaktivera enheter)</span><span class="sxs-lookup"><span data-stu-id="2b05c-171">Bulk status changes (enable or disable devices)</span></span>
* <span data-ttu-id="2b05c-172">Massinläsning tilldelning av nya nycklar för autentisering av enheten</span><span class="sxs-lookup"><span data-stu-id="2b05c-172">Bulk assignment of new device authentication keys</span></span>
* <span data-ttu-id="2b05c-173">Massinläsning auto-omskapande av enheten autentiseringsnycklar</span><span class="sxs-lookup"><span data-stu-id="2b05c-173">Bulk auto-regeneration of device authentication keys</span></span>
* <span data-ttu-id="2b05c-174">Massuppdatering av identiska data</span><span class="sxs-lookup"><span data-stu-id="2b05c-174">Bulk update of twin data</span></span>

<span data-ttu-id="2b05c-175">Du kan utföra en kombination av föregående åtgärder i en enskild **ImportDevicesAsync** anropa.</span><span class="sxs-lookup"><span data-stu-id="2b05c-175">You can perform any combination of the preceding operations within a single **ImportDevicesAsync** call.</span></span> <span data-ttu-id="2b05c-176">Du kan till exempel registrera nya enheter och ta bort eller uppdatera befintliga enheter samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2b05c-176">For example, you can register new devices and delete or update existing devices at the same time.</span></span> <span data-ttu-id="2b05c-177">När den används tillsammans med den **ExportDevicesAsync** metod, du kan migrera alla dina enheter från en IoT-hubb helt till en annan.</span><span class="sxs-lookup"><span data-stu-id="2b05c-177">When used along with the **ExportDevicesAsync** method, you can completely migrate all your devices from one IoT hub to another.</span></span>

<span data-ttu-id="2b05c-178">Om importfilen innehåller dubbla metadata, sedan dessa metadata skriver över befintliga dubbla metadata.</span><span class="sxs-lookup"><span data-stu-id="2b05c-178">If the import file includes twin metadata, then this metadata overwrites the existing twin metadata.</span></span> <span data-ttu-id="2b05c-179">Om importfilen inte innehåller dubbla metadata, sedan bara den `lastUpdateTime` metadata har uppdaterats med den aktuella tiden.</span><span class="sxs-lookup"><span data-stu-id="2b05c-179">If the import file does not include twin metadata, then only the `lastUpdateTime` metadata is updated using the current time.</span></span>

<span data-ttu-id="2b05c-180">Använd det valfria **ImportMode %** egenskap i serialiseringsdata import för varje enhet för att styra den importera processen per enheten.</span><span class="sxs-lookup"><span data-stu-id="2b05c-180">Use the optional **importMode** property in the import serialization data for each device to control the import process per-device.</span></span> <span data-ttu-id="2b05c-181">Den **ImportMode %** -egenskap har följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="2b05c-181">The **importMode** property has the following options:</span></span>

| <span data-ttu-id="2b05c-182">ImportMode %</span><span class="sxs-lookup"><span data-stu-id="2b05c-182">importMode</span></span> | <span data-ttu-id="2b05c-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2b05c-183">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2b05c-184">**createOrUpdate**</span><span class="sxs-lookup"><span data-stu-id="2b05c-184">**createOrUpdate**</span></span> |<span data-ttu-id="2b05c-185">Om en enhet inte finns med det angivna **id**, den nyligen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="2b05c-185">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="2b05c-186">Om enheten redan befintlig information skrivs över med den angivna indata utan avser den **ETag** värde.</span><span class="sxs-lookup"><span data-stu-id="2b05c-186">If the device already exists, existing information is overwritten with the provided input data without regard to the **ETag** value.</span></span> <br> <span data-ttu-id="2b05c-187">Användaren kan ange identiska data tillsammans med enhetsdata.</span><span class="sxs-lookup"><span data-stu-id="2b05c-187">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="2b05c-188">Den dubbla etag bearbetas om anges separat från enhetens etag.</span><span class="sxs-lookup"><span data-stu-id="2b05c-188">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="2b05c-189">Om det finns en felaktig matchning med befintliga dubbla etag, skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-189">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="2b05c-190">**skapa**</span><span class="sxs-lookup"><span data-stu-id="2b05c-190">**create**</span></span> |<span data-ttu-id="2b05c-191">Om en enhet inte finns med det angivna **id**, den nyligen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="2b05c-191">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="2b05c-192">Om enheten redan finns skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-192">If the device already exists, an error is written to the log file.</span></span> <br> <span data-ttu-id="2b05c-193">Användaren kan ange identiska data tillsammans med enhetsdata.</span><span class="sxs-lookup"><span data-stu-id="2b05c-193">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="2b05c-194">Den dubbla etag bearbetas om anges separat från enhetens etag.</span><span class="sxs-lookup"><span data-stu-id="2b05c-194">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="2b05c-195">Om det finns en felaktig matchning med befintliga dubbla etag, skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-195">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="2b05c-196">**Uppdatera**</span><span class="sxs-lookup"><span data-stu-id="2b05c-196">**update**</span></span> |<span data-ttu-id="2b05c-197">Om det finns redan en enhet med det angivna **id**, befintlig information skrivs över med den angivna indata utan avser den **ETag** värde.</span><span class="sxs-lookup"><span data-stu-id="2b05c-197">If a device already exists with the specified **id**, existing information is overwritten with the provided input data without regard to the **ETag** value.</span></span> <br/><span data-ttu-id="2b05c-198">Om enheten inte finns skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-198">If the device does not exist, an error is written to the log file.</span></span> |
| <span data-ttu-id="2b05c-199">**updateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="2b05c-199">**updateIfMatchETag**</span></span> |<span data-ttu-id="2b05c-200">Om det finns redan en enhet med det angivna **id**, befintlig information skrivs över med den angivna indata om det finns en **ETag** matchar.</span><span class="sxs-lookup"><span data-stu-id="2b05c-200">If a device already exists with the specified **id**, existing information is overwritten with the provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="2b05c-201">Om enheten inte finns skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-201">If the device does not exist, an error is written to the log file.</span></span> <br/><span data-ttu-id="2b05c-202">Om det finns en **ETag** matchningsfel, ett fel skrivs till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-202">If there is an **ETag** mismatch, an error is written to the log file.</span></span> |
| <span data-ttu-id="2b05c-203">**createOrUpdateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="2b05c-203">**createOrUpdateIfMatchETag**</span></span> |<span data-ttu-id="2b05c-204">Om en enhet inte finns med det angivna **id**, den nyligen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="2b05c-204">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="2b05c-205">Om enheten redan befintlig information skrivs över med den angivna indata om det finns en **ETag** matchar.</span><span class="sxs-lookup"><span data-stu-id="2b05c-205">If the device already exists, existing information is overwritten with the provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="2b05c-206">Om det finns en **ETag** matchningsfel, ett fel skrivs till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-206">If there is an **ETag** mismatch, an error is written to the log file.</span></span> <br> <span data-ttu-id="2b05c-207">Användaren kan ange identiska data tillsammans med enhetsdata.</span><span class="sxs-lookup"><span data-stu-id="2b05c-207">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="2b05c-208">Den dubbla etag bearbetas om anges separat från enhetens etag.</span><span class="sxs-lookup"><span data-stu-id="2b05c-208">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="2b05c-209">Om det finns en felaktig matchning med befintliga dubbla etag, skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-209">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="2b05c-210">**ta bort**</span><span class="sxs-lookup"><span data-stu-id="2b05c-210">**delete**</span></span> |<span data-ttu-id="2b05c-211">Om det finns redan en enhet med det angivna **id**, tas den bort utan avser den **ETag** värde.</span><span class="sxs-lookup"><span data-stu-id="2b05c-211">If a device already exists with the specified **id**, it is deleted without regard to the **ETag** value.</span></span> <br/><span data-ttu-id="2b05c-212">Om enheten inte finns skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-212">If the device does not exist, an error is written to the log file.</span></span> |
| <span data-ttu-id="2b05c-213">**deleteIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="2b05c-213">**deleteIfMatchETag**</span></span> |<span data-ttu-id="2b05c-214">Om det finns redan en enhet med det angivna **id**, raderas om det finns en **ETag** matchar.</span><span class="sxs-lookup"><span data-stu-id="2b05c-214">If a device already exists with the specified **id**, it is deleted only if there is an **ETag** match.</span></span> <span data-ttu-id="2b05c-215">Om enheten inte finns skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-215">If the device does not exist, an error is written to the log file.</span></span> <br/><span data-ttu-id="2b05c-216">Om det finns en ETag-Typfel, skrivs ett fel till loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-216">If there is an ETag mismatch, an error is written to the log file.</span></span> |

> [!NOTE]
> <span data-ttu-id="2b05c-217">Om serialiseringsdata inte explicit definiera en **ImportMode %** flaggan för en enhet, används som standard **createOrUpdate** under importen.</span><span class="sxs-lookup"><span data-stu-id="2b05c-217">If the serialization data does not explicitly define an **importMode** flag for a device, it defaults to **createOrUpdate** during the import operation.</span></span>

## <a name="import-devices-example--bulk-device-provisioning"></a><span data-ttu-id="2b05c-218">Importera enheter exempel – masskopiera enhetsetableringen</span><span class="sxs-lookup"><span data-stu-id="2b05c-218">Import devices example – bulk device provisioning</span></span>

<span data-ttu-id="2b05c-219">Följande C# kodexempel illustrerar hur du skapar flera identiteter i enheten som:</span><span class="sxs-lookup"><span data-stu-id="2b05c-219">The following C# code sample illustrates how to generate multiple device identities that:</span></span>

* <span data-ttu-id="2b05c-220">Inkludera autentiseringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="2b05c-220">Include authentication keys.</span></span>
* <span data-ttu-id="2b05c-221">Skriva enhetsinformationen till en blockblobb.</span><span class="sxs-lookup"><span data-stu-id="2b05c-221">Write that device information to a block blob.</span></span>
* <span data-ttu-id="2b05c-222">Importera enheterna till identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="2b05c-222">Import the devices into the identity registry.</span></span>

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in the Microsoft.Azure.Devices.Common namespace
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

  // Add device to the list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write the list to the blob
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

// Call import using the blob to add new devices
// Log information related to the job is written to the same container
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

## <a name="import-devices-example--bulk-deletion"></a><span data-ttu-id="2b05c-223">Importera enheter exempel – massborttagning</span><span class="sxs-lookup"><span data-stu-id="2b05c-223">Import devices example – bulk deletion</span></span>

<span data-ttu-id="2b05c-224">Följande kodexempel visar hur du tar bort de enheter som du lade till i kodexemplet:</span><span class="sxs-lookup"><span data-stu-id="2b05c-224">The following code sample shows you how to delete the devices you added using the previous code sample:</span></span>

```csharp
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
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

// Step 3: Call import using the same blob to delete all devices
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

## <a name="get-the-container-sas-uri"></a><span data-ttu-id="2b05c-225">Hämta behållaren SAS-URI</span><span class="sxs-lookup"><span data-stu-id="2b05c-225">Get the container SAS URI</span></span>

<span data-ttu-id="2b05c-226">Följande kodexempel visar hur du skapar en [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) med läsa, skriva och ta bort behörigheter för en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="2b05c-226">The following code sample shows you how to generate a [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) with read, write, and delete permissions for a blob container:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a><span data-ttu-id="2b05c-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b05c-227">Next steps</span></span>

<span data-ttu-id="2b05c-228">I den här artikeln har du lärt dig hur du utför massåtgärder mot identitetsregistret i en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2b05c-228">In this article, you learned how to perform bulk operations against the identity registry in an IoT hub.</span></span> <span data-ttu-id="2b05c-229">Du kan följa dessa länkar om du vill veta mer om hur du hanterar Azure IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="2b05c-229">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="2b05c-230">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="2b05c-230">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="2b05c-231">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="2b05c-231">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="2b05c-232">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="2b05c-232">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="2b05c-233">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="2b05c-233">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="2b05c-234">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="2b05c-234">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
