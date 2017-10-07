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
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a>Hantera din IoT-hubb enheten identiteter i grupp

Varje IoT-hubben har en identitetsregistret som du kan använda toocreate per enhet resurser i hello-tjänsten. Hej identitetsregistret kan du också toocontrol åtkomst toohello enheten riktade slutpunkter. Den här artikeln beskriver hur tooimport och exportera enheten identiteter i masskopiera tooand från en identitetsregistret.

Import och export-åtgärder sker hello gäller *jobb* som gör att du tooexecute service massåtgärder mot en IoT-hubb.

Hej **RegistryManager** klassen innehåller hello **ExportDevicesAsync** och **ImportDevicesAsync** metoder som använder hello **jobbet** Framework. Dessa metoder kan du tooexport, importera och synkronisera hello hela identitetsregistret en IoT-hubb.

## <a name="what-are-jobs"></a>Vad är jobb?

Identitet registret åtgärderna använder hello **jobbet** system när hello igen:

* Har en lång körningstid jämfört med toostandard körning åtgärder.
* Returnerar en stor mängd data toohello användare.

I stället för ett enda API-anrop som väntar på eller blockerar hello resultatet av åtgärden hello hello igen asynkront skapar en **jobbet** för att IoT-hubb. hello igen och sedan omedelbart returnerar en **JobProperties** objekt.

Följande C# hello code fragment visas hur toocreate ett exportjobb:

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> toouse hello **RegistryManager** klassen i C#-kod, lägga till hello **Microsoft.Azure.Devices** NuGet-paketet tooyour projekt. Hej **RegistryManager** klassen är i hello **Microsoft.Azure.Devices** namnområde.

Du kan använda hello **RegistryManager** klassen tooquery hello tillstånd hello **jobbet** med hello returnerade **JobProperties** metadata.

hello visar följande kodfragment för C# hur toopoll var femte sekund toosee om hello jobbet har slutförts körs:

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

## <a name="export-devices"></a>Exportera enheter

Använd hello **ExportDevicesAsync** metoden tooexport hello helhet för en IoT-hubb identitet registret tooan [Azure Storage](../storage/index.md) blob-behållare med en [signatur för delad åtkomst](../storage/common/storage-security-guide.md#data-plane-security).

Den här metoden kan du toocreate tillförlitliga säkerhetskopior av din enhetsinformation i en blobbbehållare som du bestämmer.

Hej **ExportDevicesAsync** kräver två parametrar:

* En *sträng* som innehåller en URI för en blobbbehållare. Den här URI måste innehålla en SAS-token som ger skrivbehörighet toohello behållare. hello jobbet skapar en blockblobb i den här behållaren toostore hello serialiseras exportera enhetsdata. hello SAS-token måste innehålla följande behörigheter:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* En *booleskt* som anger om du vill tooexclude autentiseringsnycklar exportera data. Om **FALSKT**, autentiseringsnycklar ingår i utdata för export. I annat fall exporteras nycklar som **null**.

hello visar följande kodfragment för C# hur tooinitiate ett exportjobb som innehåller enheten autentiseringsnycklar i hello exportera data och sedan söka efter slutförande:

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

hello jobbet lagrar utdata i hello tillhandahålls blob-behållaren som en blockblobb med hello namnet **devices.txt**. hello utdata består av JSON serialiserade data på enheten, med en enhet per rad.

hello visar följande exempel hello utdata:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Om en enhet har identiska data, exporteras också hello identiska data tillsammans med hello data på enheten. hello visar följande exempel det här formatet. Alla data från hello ”twinETag” rad fram hello slutet är identiska data.

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

Om du behöver komma åt toothis data i koden, kan du enkelt deserialisera dessa data med hello **ExportImportDevice** klass. hello visar följande kodfragment för C# hur tooread enhetsinformation som var tidigare exporterade tooa blockblob:

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
> Du kan också använda hello **GetDevicesAsync** metod för hello **RegistryManager** klassen toofetch en lista över dina enheter. Den här metoden har dock en hård fjärrskrivbordsanslutning 1000 hello antalet enhetsobjekt som ska returneras. hello förväntades användningsfall för hello **GetDevicesAsync** metoden är för utveckling scenarier tooaid felsökning och rekommenderas inte för produktionsarbetsbelastningar.

## <a name="import-devices"></a>Importera enheter

Hej **ImportDevicesAsync** metod i hello **RegistryManager** klassen kan du tooperform import och synkronisering massåtgärder i en IoT-hubb identitetsregistret. Som hello **ExportDevicesAsync** metod, hello **ImportDevicesAsync** metoden använder hello **jobbet** framework.

Var noga med hello **ImportDevicesAsync** metoden eftersom i tillägg tooprovisioning nya enheter i din identitetsregistret, kan den också uppdatera och ta bort befintliga enheter.

> [!WARNING]
> En importen kan inte ångras. Säkerhetskopiera dina befintliga data med hjälp av hello alltid **ExportDevicesAsync** metoden tooanother blob-behållare innan du gör bulk ändras tooyour identitetsregistret.

Hej **ImportDevicesAsync** metoden har två parametrar:

* En *sträng* som innehåller en URI för en [Azure Storage](../storage/index.md) blob-behållaren toouse som *inkommande* toohello jobb. Den här URI måste innehålla en SAS-token som ger läsbehörighet toohello behållare. Den här behållaren måste innehålla en blob med hello namnet **devices.txt** som innehåller hello serialiseras enheten data tooimport till din identitetsregistret. hello importera data måste innehålla enhetsinformation i hello samma JSON-format som hello **ExportImportDevice** jobbet används när den skapar en **devices.txt** blob. hello SAS-token måste innehålla följande behörigheter:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* En *sträng* som innehåller en URI för en [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob-behållaren toouse som *utdata* hello-jobbet. hello jobbet skapar en blockblobb i den här behållaren toostore den felinformation från hello slutförd import **jobbet**. hello SAS-token måste innehålla följande behörigheter:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> hello två parametrar kan peka toohello samma blob-behållare. hello separat parametrar kan bara mer kontroll över dina data eftersom hello utdata behållare kräver ytterligare behörighet.

Följande C# hello code fragment visas hur tooinitiate importen:

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Den här metoden kan också vara används tooimport hello data för hello enheten dubbla. hello format för hello-indata är hello samma som hello-format som visas i hello **ExportDevicesAsync** avsnitt. På så sätt kan importera du hello exporterade data. Hej **$metadata** är valfritt.

## <a name="import-behavior"></a>Importera beteende

Du kan använda hello **ImportDevicesAsync** metoden tooperform hello följande Massredigera åtgärder i identitetsregistret:

* Massinläsning registrering av nya enheter
* Massinläsning borttagningar av befintliga enheter
* Massredigera statusändringar (aktivera eller inaktivera enheter)
* Massinläsning tilldelning av nya nycklar för autentisering av enheten
* Massinläsning auto-omskapande av enheten autentiseringsnycklar
* Massuppdatering av identiska data

Du kan utföra en kombination av hello föregående åtgärder i en enda **ImportDevicesAsync** anropa. Exempelvis kan du registrera nya enheter och ta bort eller uppdatera befintliga enheter på hello samtidigt. När den används tillsammans med hello **ExportDevicesAsync** metod du helt kan migrera alla dina enheter från en IoT-hubb tooanother.

Om hello importfilen innehåller dubbla metadata, skrivs dessa metadata hello befintliga dubbla metadata. Om hello importfilen inte innehåller dubbla metadata sedan endast hello `lastUpdateTime` metadata uppdateras med hjälp av hello aktuell tid.

Använd hello valfria **ImportMode %** egenskap i hello importera serialiseringsdata för varje enhet toocontrol hello importera processen per enhet. Hej **ImportMode %** -egenskap har hello följande alternativ:

| ImportMode % | Beskrivning |
| --- | --- |
| **createOrUpdate** |Om en enhet inte finns med hello anges **id**, den nyligen har registrerats. <br/>Om hello enheten redan befintlig information skrivs över med hello angivna indata utan hänsyn toohello **ETag** värde. <br> hello användare kan du ange identiska data tillsammans med hello data på enheten. hello dubbla etag bearbetas om anges separat från hello enhetens etag. Om det finns en felaktig matchning med hello befintliga dubbla etag, skrivs ett fel toohello loggfilen. |
| **skapa** |Om en enhet inte finns med hello anges **id**, den nyligen har registrerats. <br/>Om hello enheten redan finns skrivs ett fel toohello loggfilen. <br> hello användare kan du ange identiska data tillsammans med hello data på enheten. hello dubbla etag bearbetas om anges separat från hello enhetens etag. Om det finns en felaktig matchning med hello befintliga dubbla etag, skrivs ett fel toohello loggfilen. |
| **Uppdatera** |Om det finns redan en enhet med hello anges **id**, befintlig information skrivs över med hello angivna indata utan hänsyn toohello **ETag** värde. <br/>Om hello enheten inte finns, är ett fel skrivs toohello loggfilen. |
| **updateIfMatchETag** |Om det finns redan en enhet med hello anges **id**, befintlig information skrivs över med hello angivna indata, om det finns en **ETag** matchar. <br/>Om hello enheten inte finns, är ett fel skrivs toohello loggfilen. <br/>Om det finns en **ETag** matchningsfel, ett fel skrivs toohello loggfilen. |
| **createOrUpdateIfMatchETag** |Om en enhet inte finns med hello anges **id**, den nyligen har registrerats. <br/>Om hello enheten redan befintlig information skrivs över med hello angivna indata, om det finns en **ETag** matchar. <br/>Om det finns en **ETag** matchningsfel, ett fel skrivs toohello loggfilen. <br> hello användare kan du ange identiska data tillsammans med hello data på enheten. hello dubbla etag bearbetas om anges separat från hello enhetens etag. Om det finns en felaktig matchning med hello befintliga dubbla etag, skrivs ett fel toohello loggfilen. |
| **ta bort** |Om det finns redan en enhet med hello anges **id**, det har tagits bort utan hänsyn toohello **ETag** värde. <br/>Om hello enheten inte finns, är ett fel skrivs toohello loggfilen. |
| **deleteIfMatchETag** |Om det finns redan en enhet med hello anges **id**, raderas om det finns en **ETag** matchar. Om hello enheten inte finns, är ett fel skrivs toohello loggfilen. <br/>Om det finns en ETag-Typfel, skrivs ett fel toohello loggfilen. |

> [!NOTE]
> Om hello serialiseringsdata inte explicit definiera en **ImportMode %** flaggan för en enhet, används som standard för**createOrUpdate** under hello importåtgärden.

## <a name="import-devices-example--bulk-device-provisioning"></a>Importera enheter exempel – masskopiera enhetsetableringen

hello följande C# kodexempel illustrerar hur toogenerate flera identiteter i enheten som:

* Inkludera autentiseringsnycklar.
* Skriva den enheten information tooa blockblob.
* Importera hello enheter till hello identitetsregistret.

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

## <a name="import-devices-example--bulk-deletion"></a>Importera enheter exempel – massborttagning

hello visar följande kodexempel hur toodelete hello enheter som du lade till i hello kodexemplet:

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

## <a name="get-hello-container-sas-uri"></a>Hämta hello behållarens SAS-URI

hello följande kodexempel visar hur toogenerate en [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) med läsa, skriva och ta bort behörigheter för en blob-behållare:

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

## <a name="next-steps"></a>Nästa steg

I den här artikeln har du lärt dig hur tooperform Massredigera åtgärder mot hello identitetsregistret i en IoT-hubb. Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:

* [IoT-hubb mått][lnk-metrics]
* [Åtgärder som övervakning][lnk-monitor]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med IoT kant][lnk-iotedge]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
