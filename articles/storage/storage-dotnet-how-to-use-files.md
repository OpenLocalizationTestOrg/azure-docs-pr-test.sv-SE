---
title: "aaaDevelop för Azure File storage med .NET | Microsoft Docs"
description: "Lär dig hur toodevelop .NET-program och tjänster som använder Azure File storage toostore fildata."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: aa8f84f1c93249055370e2a2cb33d7118b972be1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-net"></a>Utveckla för Azure File Storage med .NET 
> [!NOTE]
> Den här artikeln visar hur toomanage Azure File storage med .NET-kod. toolearn mer om Azure File storage finns hello [introduktion tooAzure fillagring](storage-files-introduction.md).
>

[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar hello grunderna i toodevelop .NET-program eller tjänster som använder Azure File storage toostore fildata. I den här kursen ska vi skapa ett enkelt konsolprogram och visa hur tooperform grundläggande åtgärder med .NET och Azure File storage:

* Hämta hello innehållet i en fil
* Ange hello kvot (största tillåtna storleken) för hello filresurs.
* Skapa en signatur för delad åtkomst (SAS-nyckel) för en fil som använder en princip för delad åtkomst som definierats på hello-resurs.
* Kopiera en fil för tooanother i hello samma lagringskonto.
* Kopiera en fil tooa blob i hello samma lagringskonto.
* Använd Azure Storage-mätvärden för felsökning

> [!Note]  
> Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard System.IO klasser för i/o. Den här artikeln beskriver hur toowrite program som använder hello Azure Storage .NET SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage. 


## <a name="create-hello-console-application-and-obtain-hello-assembly"></a>Skapa hello konsolprogram och få hello sammansättning
Skapa ett nytt Windows-konsolprogram i Visual Studio. hello följande steg visar hur toocreate ett konsolprogram i Visual Studio 2017 dock hello stegen är ungefär i andra versioner av Visual Studio.

1. Välj **Arkiv** > **Nytt** > **Projekt**
2. Välj **Installerat** > **Mallar** > **Visual C#** > **Windows Classic Desktop**
3. Välj **Konsolprogram (.NET Framework)**
4. Ange ett namn för ditt program i hello **Name:** fält
5. Välj **OK**

Alla kodexempel i den här kursen kan läggas till toohello `Main()` -metoden i konsolen programmets `Program.cs` fil.

Du kan använda hello Azure Storage-klientbibliotek för någon typ av .NET-program, inklusive en Azure cloud service eller web app, och stationära och mobila program. I den här guiden använder vi oss av en konsolapp för enkelhetens skull.

## <a name="use-nuget-tooinstall-hello-required-packages"></a>Använd NuGet tooinstall krävs hello-paket
Det finns två paket som du behöver tooreference i ditt projekt toocomplete för den här kursen:

* [Microsoft Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): det här paketet ger programmatisk åtkomst toodata resurser i ditt lagringskonto.
* [Microsoft Azure Configuration Manager-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): det här paketet tillhandahåller en klass för parsning av en anslutningssträng i en konfigurationsfil, oavsett var ditt program körs.

Du kan använda NuGet tooobtain båda paketen. Följ de här stegen:

1. Högerklicka på ditt projekt i **Solution Explorer** och välj **Hantera NuGet-paket**.
2. Sök online efter ”WindowsAzure.Storage” och klicka på **installera** tooinstall hello Storage-klientbiblioteket och dess beroenden.
3. Sök online efter ”WindowsAzure.ConfigurationManager” och klicka på **installera** tooinstall hello Azure Configuration Manager.

## <a name="save-your-storage-account-credentials-toohello-appconfig-file"></a>Spara storage-konto autentiseringsuppgifter toohello app.config-filen
Nu ska du spara dina autentiseringsuppgifter i projektets app.config-fil. Redigera hello app.config-filen så att den visas liknande toohello följande exempel, ersätter `myaccount` med namnet på ditt lagringskonto, och `mykey` med din lagringskontonyckel.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> hello senaste versionen av hello Azure storage-emulatorn har inte stöd för Azure File storage. Anslutningssträngen måste ha ett Azure Storage-konto i hello molnet toowork med Azure File storage.

## <a name="add-using-directives"></a>Lägga till med hjälp av direktiv
Öppna hello `Program.cs` från Solution Explorer och Lägg till följande hello med direktiven toohello överkant hello-filen.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-hello-file-share-programmatically"></a>Åtkomst hello filresursen via programmering
Lägg till följande kod toohello hello `Main()` metod (efter hello koden som visas ovan) tooretrieve hello anslutningssträngen. Den här koden hämtar en referens toohello fil som vi skapade tidigare och matar ut sitt innehåll toohello konsolfönster.

```csharp
// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello file exists.
        if (file.Exists())
        {
            // Write hello contents of hello file toohello console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Kör hello konsolens programmet toosee hello utdata.

## <a name="set-hello-maximum-size-for-a-file-share"></a>Ange hello största storleken för en filresurs
Från och med version 5.x av hello Azure Storage-klientbibliotek, du kan ange set hello kvot eller en maximal storlek för en filresurs, i gigabyte. Du kan också kontrollera hur mycket data som lagras på resursen hello toosee.

Genom att ange hello kvoten för en resurs, kan du begränsa hello totala storleken för hello-filer som lagras på hello-resurs. Om hello totala storleken på filerna på hello resursen överskrider ange hello kvot för hello resursen och klienter ska tooincrease hello storleken på befintliga filer eller skapa nya filer såvida inte dessa filer är tomma.

hello exemplet nedan visar hur toocheck hello aktuella användningen av en resurs och hur tooset hello kvoten för hello resursen.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Check current usage stats for hello share.
    // Note that hello ShareStats object is part of hello protocol layer for hello File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify hello maximum size of hello share, in GB.
    // This line sets hello quota toobe 10 GB greater than hello current usage of hello share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check hello quota for hello share. Call FetchAttributes() toopopulate hello share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Generera en signatur för delad åtkomst för en fil eller filresurs
Från och med version 5.x av hello Azure Storage-klientbibliotek, du kan generera en signatur för delad åtkomst (SAS) för en filresurs eller för en enskild fil. Du kan också skapa en princip för delad åtkomst på en toomanage delade filresurser signaturer. Skapa en princip för delad åtkomst rekommenderas eftersom det ger dig möjlighet att återkalla hello SAS om det behövs.

hello följande exempel skapar en princip för delad åtkomst på en resurs och använder sedan som delar tooprovide hello principbegränsningar för en SAS för en fil i hello.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for hello share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add hello shared access policy toohello share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in hello share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from hello SAS, and write some text toohello file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

Mer information om hur du skapar och använder signaturer för delad åtkomst finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md) och [Skapa och använda en SAS med Azure Blobs](storage-dotnet-shared-access-signature-part-2.md).

## <a name="copy-files"></a>Kopiera filer
Från och med version 5.x av hello Azure Storage-klientbibliotek, kan du kopiera en fil för tooanother, en tooa blob-fil eller en tooa blob-fil. I nästa avsnitt hello visar vi hur tooperform dessa kopiera operations programmässigt.

Du kan också använda AzCopy toocopy en fil tooanother eller toocopy en blob tooa fil eller tvärtom. Se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).

> [!NOTE]
> Om du kopierar en tooa blob-fil eller en fil tooa blob, måste du använda en delad åtkomst (SAS)-signaturen tooauthenticate hello källobjektet, även om du kopierar inom hello samma lagringskonto.
> 
> 

**Kopiera en fil tooanother** hello följande exempel kopierar en fil för tooanother i hello samma resurs. Eftersom den här kopieringsåtgärden kopierar mellan filer i hello samma lagringskonto kan du använda delad nyckel autentisering tooperform hello kopia.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference toohello destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start hello copy operation.
            destFile.StartCopy(sourceFile);

            // Write hello contents of hello destination file toohello console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

**Kopiera en fil tooa blob** hello följande exempel skapas en fil och kopierar den tooa blob inom hello samma lagringskonto. hello exemplet skapas en SAS för källfilen hello, vilka hello-tjänsten använder tooauthenticate åtkomst toohello källfilen under kopieringen hello.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in hello root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in hello root directory.");

// Get a reference toohello blob toowhich hello file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for hello file that's valid for 24 hours.
// Note that when you are copying a file tooa blob, or a blob tooa file, you must use a SAS
// tooauthenticate access toohello source object, even if you are copying within hello same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for hello source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct hello URI toohello source file, including hello SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy hello file toohello blob.
destBlob.StartCopy(fileSasUri);

// Write hello contents of hello file toohello console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

Du kan kopiera en tooa blob-fil i hello samma sätt. Om hello källobjektet är en blobb skapar du en SAS tooauthenticate åtkomst toothat blob under hello kopieringen.

## <a name="troubleshooting-azure-file-storage-using-metrics"></a>Felsöka Azure File Storage med hjälp av mätvärden
Nu stöder Azure Storage Analytics mätvärden för Azure File Storage. Med hjälp av mätvärdesdata kan du spåra begäranden och diagnostisera problem.


Du kan aktivera mätvärden för Azure File storage från hello [Azure Portal](https://portal.azure.com). Du kan också aktivera mätvärden via programmering genom att anropa hello åtgärden Ange egenskaper för filtjänsten via hello REST-API, eller en av dess motsvarigheter i hello Storage-klientbiblioteket.


hello följande kodexempel visar hur toouse hello Storage-klientbibliotek för .NET tooenable mätvärden för Azure File storage.

Lägg först till hello följande `using` direktiven tooyour `Program.cs` filen dessutom toothose du lades till ovan:

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Observera att medan Azure BLOB, Azure Table och Azure köer använder hello delade `ServiceProperties` typen i hello `Microsoft.WindowsAzure.Storage.Shared.Protocol` namnområde, Azure File storage använder sin egen typ hello `FileServiceProperties` typen i hello `Microsoft.WindowsAzure.Storage.File.Protocol` namnområde. Båda namnområden måste refereras från din kod, men för hello följande kod toocompile.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create hello File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that hello File service currently uses its own service properties type,
// available in hello Microsoft.WindowsAzure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read hello metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

Dessutom kan du referera för[Azure File storage felsökning artikel](storage-troubleshoot-file-connection-problems.md) för slutpunkt till slutpunkt felsökningsinformation.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure File Storage finns på följande länkar.

### <a name="conceptual-articles-and-videos"></a>Begreppsrelaterade artiklar och videoklipp
* [Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Hur toouse Azure File storage med Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Verktygsstöd för File Storage
* [Använd Azure PowerShell med Azure Storage](storage-powershell-guide-full.md)
* [Hur toouse AzCopy med Microsoft Azure Storage](storage-use-azcopy.md)
* [Använda hello Azure CLI med Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Felsökning av problem i Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Referens
* [Storage-klientbibliotek för .NET-referens](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [File Service REST API referens](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Blogginlägg
* [Azure File Storage finns nu allmänt tillgänglig](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inuti Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introduktion till Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Spara anslutningar tooMicrosoft Azure File storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
