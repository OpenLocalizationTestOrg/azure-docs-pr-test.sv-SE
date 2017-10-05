---
title: "Utveckla för Azure File Storage med .NET | Microsoft Docs"
description: "Lär dig hur du utvecklar .NET-program och tjänster som använder Azure File Storage för att lagra fildata."
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
ms.openlocfilehash: e26da88ef1803d47d7286c5ae836ac9c041dadd1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-net"></a>Utveckla för Azure File Storage med .NET 
> [!NOTE]
> Den här artikeln visar hur du hanterar Azure File Storage med .NET-kod. Mer information om Azure File Storage finns i [Introduction to Azure File storage](storage-files-introduction.md) (Introduktion till Azure File Storage).
>

[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar grunderna i hur du använder .NET för att utveckla program eller tjänster som lagrar fildata med hjälp av Azure File Storage. I den här självstudiekursen skapar vi ett enkelt konsolprogram och visar hur du utför grundläggande åtgärder med .NET och Azure File Storage:

* Hämta innehållet i en fil
* Ange kvoten (den största tillåtna storleken) för filresursen.
* Skapa en signatur för delad åtkomst (SAS-nyckel) för en fil som använder en princip för delad åtkomst som definierats för resursen.
* Kopiera en fil till en annan fil i samma lagringskonto.
* Kopiera en fil till en blobb i samma lagringskonto.
* Använd Azure Storage-mätvärden för felsökning

> [!Note]  
> Eftersom Azure File Storage kan nås över SMB kan du skriva enkla program som har åtkomst till Azure-filresursen med System.IO-standardklasser för fil-I/O. Den här artikeln visar hur du skriver program som använder Azure Storage .NET SDK, som använder [REST-API:et för Azure File Storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) för att kommunicera med Azure File Storage. 


## <a name="create-the-console-application-and-obtain-the-assembly"></a>Skapa konsolprogrammet och hämta monteringen
Skapa ett nytt Windows-konsolprogram i Visual Studio. Följande steg beskriver hur du skapar ett konsolprogram i Visual Studio 2017, men stegen är liknande i andra versioner av Visual Studio.

1. Välj **Arkiv** > **Nytt** > **Projekt**
2. Välj **Installerat** > **Mallar** > **Visual C#** > **Windows Classic Desktop**
3. Välj **Konsolprogram (.NET Framework)**
4. Ange ett namn för ditt program i fältet **Namn**
5. Välj **OK**

Alla kodexempel i den här självstudiekursen kan läggas till i `Main()`-metoden i konsolprogrammets `Program.cs`-fil.

Du kan använda Azure Storage-klientbiblioteket i alla typer av .NET-program, t.ex. en Azure-molntjänst eller webbapp, eller i dator- och mobilprogram. I den här guiden använder vi oss av en konsolapp för enkelhetens skull.

## <a name="use-nuget-to-install-the-required-packages"></a>Använd NuGet för att installera de paket som behövs
Det finns två paket som du måste referera till i ditt projekt för att slutföra den här kursen:

* [Microsoft Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): det här paketet ger programmatisk åtkomst till dataresurser i ditt lagringskonto.
* [Microsoft Azure Configuration Manager-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): det här paketet tillhandahåller en klass för parsning av en anslutningssträng i en konfigurationsfil, oavsett var ditt program körs.

Du kan använda NuGet för att hämta båda paketen. Följ de här stegen:

1. Högerklicka på ditt projekt i **Solution Explorer** och välj **Hantera NuGet-paket**.
2. Sök online efter ”WindowsAzure.Storage” och klicka på **Installera** för att installera Storage-klientbiblioteket och alla dess beroenden.
3. Sök online efter ”WindowsAzure.ConfigurationManager” och klicka på **Installera** för att installera Azure Configuration Manager.

## <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a>Spara autentiseringsuppgifterna för ditt lagringskonto i app.config-filen
Nu ska du spara dina autentiseringsuppgifter i projektets app.config-fil. Redigera app.config-filen så att den ser ut som i följande exempel. Ersätt `myaccount` med namnet på ditt lagringskonto och `mykey` med din åtkomstnyckel för lagring.

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
> Den senaste versionen av Azure Storage-emulatorn har inte stöd för Azure File Storage. Anslutningssträngen måste peka på ett Azure Storage-konto i molnet för att fungera med Azure File Storage.

## <a name="add-using-directives"></a>Lägga till med hjälp av direktiv
Öppna filen `Program.cs` från Solution Explorer och lägg till följande med hjälp av direktiv överst i filen.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-the-file-share-programmatically"></a>Ansluta till filresursen via programmering
Lägg till följande kod i `Main()`-metoden (efter koden som visas ovan) för att hämta anslutningssträngen. Den här koden hämtar en referens till den fil som vi skapade tidigare och returnerar filens innehåll i konsolfönstret.

```csharp
// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Visa resultatet genom att köra konsolprogrammet.

## <a name="set-the-maximum-size-for-a-file-share"></a>Ange den största storleken för en filresurs
Från och med version 5.x av klientbiblioteket för Azure Storage kan du ange kvoten (eller den största storleken) för en filresurs, i antal gigabyte. Du kan också kontrollera hur mycket data som lagras på resursen för närvarande.

Genom att ange kvoten för en resurs kan du begränsa den totala storleken på filerna som lagras på resursen. Om den totala storleken på filerna som lagras på resursen överskrider kvoten som angetts för resursen kan klienterna inte öka storleken på befintliga filer eller skapa nya filer såvida inte dessa filer är tomma.

Exemplet nedan visar hur du kontrollerar användningen av en resurs och hur du ställer in kvoten för resursen.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Generera en signatur för delad åtkomst för en fil eller filresurs
Från och med version 5.x av klientbiblioteket för Azure Storage kan du generera en signatur för delad åtkomst (SAS) för en filresurs eller för en enskild fil. Du kan också skapa en princip för delad åtkomst på en filresurs för att hantera signaturer för delad åtkomst. Vi rekommenderar att du skapar en princip för delad åtkomst eftersom det ger dig möjlighet att återkalla signaturen för delad åtkomst om det behövs.

I följande exempel skapar vi en princip för delad åtkomst på en resurs och använder sedan principen för att ange begränsningarna för en signatur för delad åtkomst på en fil i resursen.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

Mer information om hur du skapar och använder signaturer för delad åtkomst finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md) och [Skapa och använda en SAS med Azure Blobs](storage-dotnet-shared-access-signature-part-2.md).

## <a name="copy-files"></a>Kopiera filer
Från och med version 5.x av klientbiblioteket för Azure Storage kan du kopiera en fil till en annan fil, en fil till en blobb eller en blobb till en fil. I nästa avsnitt ser du hur du utför kopieringsåtgärderna genom programmering.

Du kan också använda AzCopy för att kopiera en fil till en annan eller för att kopiera en blobb till en fil eller tvärtom. Mer information finns i [Överföra data med kommandoradsverktyget AzCopy](storage-use-azcopy.md).

> [!NOTE]
> Om du kopierar en blobb till en fil eller en fil till en blobb måste du använda en signatur för delad åtkomst (SAS) för att autentisera källobjektet, även om du kopierar inom samma lagringskonto.
> 
> 

**Kopiera en fil till en annan fil** I följande exempel kopierar vi en fil till en annan fil i samma resurs. Eftersom den här kopieringsåtgärden kopierar mellan filer i samma lagringskonto kan du använda autentisering med delad nyckel för att utföra kopieringen.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

**Kopiera en fil till en blob** I följande exempel skapar vi en fil och kopierar den till en blob inom samma lagringskonto. I exemplet skapas en SAS för källfilen, som tjänsten använder för att autentisera åtkomsten till källfilen under kopieringen.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authenticate access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

Du kan kopiera en blobb till en fil på samma sätt. Om källobjektet är en blobb skapar du en SAS för att autentisera åtkomsten till blobben under kopieringen.

## <a name="troubleshooting-azure-file-storage-using-metrics"></a>Felsöka Azure File Storage med hjälp av mätvärden
Nu stöder Azure Storage Analytics mätvärden för Azure File Storage. Med hjälp av mätvärdesdata kan du spåra begäranden och diagnostisera problem.


Du kan aktivera mätvärden för Azure File Storage från [Azure Portal](https://portal.azure.com). Du kan också aktivera mätvärden via programmering genom att anropa åtgärden Ange egenskaper för filtjänsten via REST-API:et eller någon av dess motsvarigheter i klientbiblioteket för Azure Storage.


Följande exempel visar hur du använder Azure Storage-klientbiblioteket för .NET för att aktivera mätvärden för Azure File Storage.

Lägg först till följande `using`-direktiv i `Program.cs`-filen, förutom de som du lade till ovan:

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Observera att Azure Blobs, Azure Table och Azure Queues använder den delade `ServiceProperties`-typen i namnrymden `Microsoft.WindowsAzure.Storage.Shared.Protocol` och att Azure File Storage använder en egen typ, det vill säga typen `FileServiceProperties` i namnrymden `Microsoft.WindowsAzure.Storage.File.Protocol`. Din kod måste dock referera till båda namnrymderna för att följande kod ska kompileras.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.WindowsAzure.Storage.File.Protocol namespace.
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

// Read the metrics properties we just set.
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

Du kan också gå till [Felsökningsartikeln om Azure File Storage](storage-troubleshoot-file-connection-problems.md) för felsökningsinformation från slutpunkt till slutpunkt.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure File Storage finns på följande länkar.

### <a name="conceptual-articles-and-videos"></a>Begreppsrelaterade artiklar och videoklipp
* [Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Använd Azure File Storage med Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Verktygsstöd för File Storage
* [Använd Azure PowerShell med Azure Storage](storage-powershell-guide-full.md)
* [Använd AzCopy med Microsoft Azure Storage](storage-use-azcopy.md)
* [Använd Azure CLI:et med Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Felsökning av problem i Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Referens
* [Storage-klientbibliotek för .NET-referens](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [File Service REST API referens](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Blogginlägg
* [Azure File Storage finns nu allmänt tillgänglig](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inuti Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introduktion till Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Bevara anslutningar till Microsoft Azure File Storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)